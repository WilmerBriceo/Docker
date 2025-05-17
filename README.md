## **Máquina de Escribir**

En este ejemplo se crea una animación de texto tipo *"máquina de escribir"* usando Bash dentro de un contenedor Docker muy ligero basado en Alpine Linux.

## 1. **Crear el script con `cat`**

```bash
cat << 'EOF' > typewriter.sh
#!/bin/bash
mensaje="¡Hola desde un contenedor Docker animado en la terminal!"
velocidad=0.05

for ((i=0; i<${#mensaje}; i++)); do
  echo -n "${mensaje:$i:1}"
  sleep $velocidad
done

echo ""
EOF
```

### ¿Qué hace esto?

1. **`cat << 'EOF' > typewriter.sh`**: Usa redirección para crear un archivo llamado `typewriter.sh` y escribir en él el contenido que va entre `EOF` y `EOF`. La comilla simple (`'EOF'`) impide la expansión de variables durante la escritura.

2. **`#!/bin/bash`**: Indica que el script debe ejecutarse con Bash.

3. **`mensaje="..."`**: Variable que contiene el mensaje que se va a imprimir "letra por letra".

4. **`velocidad=0.05`**: Tiempo (en segundos) de espera entre cada carácter (50 milisegundos).

5. **`for ((i=0; i<${#mensaje}; i++))`**: Bucle que recorre carácter por carácter la cadena.

6. **`echo -n "${mensaje:$i:1}"`**: Imprime el carácter actual sin salto de línea.

7. **`sleep $velocidad`**: Espera antes de imprimir el siguiente carácter.

8. **`echo ""`**: Salto de línea al final del mensaje.

**Resultado:** Cuando se ejecuta, el texto se imprime como si alguien lo estuviera escribiendo en tiempo real.

---

## 2. **Crear el Dockerfile**

```bash
cat << 'EOF' > Dockerfile
FROM alpine:latest

RUN apk add --no-cache bash

COPY typewriter.sh /typewriter.sh
RUN chmod +x /typewriter.sh

CMD ["/typewriter.sh"]
EOF
```

### Explicación línea por línea:

1. **`FROM alpine:latest`**: Usa Alpine Linux como base (una distribución extremadamente ligera y rápida).

2. **`RUN apk add --no-cache bash`**: Instala Bash, ya que Alpine viene con `sh` por defecto. `--no-cache` evita guardar archivos temporales de instalación.

3. **`COPY typewriter.sh /typewriter.sh`**: Copia el script Bash al contenedor.

4. **`RUN chmod +x /typewriter.sh`**: Da permisos de ejecución al script.

5. **`CMD ["/typewriter.sh"]`**: Indica el comando predeterminado que se ejecutará cuando se inicie el contenedor (el script de animación).

---

## 3. **Construir la imagen Docker**

```bash
docker build -t animacion-texto .
```

### ¿Qué hace?

* **`docker build`**: Construye una imagen desde el `Dockerfile`.
* **`-t animacion-texto`**: Le da el nombre `animacion-texto` a la imagen.
* **`.`**: Indica que la construcción se realiza en el directorio actual (donde están el `Dockerfile` y el `typewriter.sh`).

---

## 4. **Ejecutar el contenedor**

```bash
docker run --rm animacion-texto
```

### ¿Qué hace?

* **`docker run`**: Ejecuta un contenedor basado en la imagen.
* **`--rm`**: Elimina el contenedor automáticamente cuando termina.
* **`animacion-texto`**: Es la imagen que creamos antes.

**Resultado final:** Cuando ejecutas este comando, verás en tu terminal cómo se imprime el mensaje carácter por carácter, simulando una animación tipo máquina de escribir.
