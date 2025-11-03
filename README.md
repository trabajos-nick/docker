 ---
 title: "Informe Técnico: Uso de Docker"
 ---
 %% [markdown]
 # Informe Técnico: Uso de Docker
 **Autor:**  
 **Formato:** Informe técnico tipo README.md  
 
 ---
 
 ## Contenido del informe
 
 - Resumen de los conceptos aprendidos.  
 - Reflexiones personales (ventajas, desafíos, uso práctico).  
 - Ejemplo práctico o mini proyecto con Docker.  
 - Recursos adicionales y enlaces de consulta.  
 - Repositorio de GitHub para revisión.  
 
 ---
 %% [markdown]
 ## Aprende a usar Docker: Tutorial para principiantes
 
 Si trabajas o vas a trabajar en desarrollo de software, DevOps o infraestructura, es importante aprender Docker.  
 Este entorno permite instalar dependencias rápidamente, simplificar despliegues y reducir errores.
 
 En este informe aprenderás cómo crear, administrar y orquestar contenedores y sus dependencias.
 %% [markdown]
 ## Requisitos
 - Computadora con mínimo **2 GB** de almacenamiento libre en disco.
 - Conexión a Internet (para descargar imágenes y dependencias).
 - Acceso a **Docker Hub**.
 %% [markdown]
 ## Concepto general
 
 **¿Qué es un contenedor?**  
 Es una forma de empaquetar aplicaciones y proyectos para hacerlos portables y fáciles de desplegar.  
 
 Los contenedores se almacenan en **repositorios** (públicos o privados).  
 Antes de Docker, los desarrolladores debían ajustar manualmente versiones y dependencias.  
 Con Docker, el entorno se define una vez y se replica de forma automática en todos los equipos.
 %% [markdown]
 ## Imágenes
 
 Una **imagen Docker** es el empaquetado que contiene el código y sus dependencias.  
 De una imagen se pueden crear múltiples contenedores.
 
 ## Virtualización
 
 Docker funciona como una máquina virtual ligera.  
 A diferencia de las VMs tradicionales, los contenedores comparten el kernel del sistema operativo, haciéndolos más rápidos y eficientes.
 %% [markdown]
 ## Docker Desktop
 Es la herramienta gráfica que permite gestionar imágenes, contenedores y redes Docker desde un entorno visual.
 %% [markdown]
 ## Comandos básicos
 
 ```bash
 !docker images           # Ver imágenes disponibles
 !docker pull node        # Descargar imagen de Node.js
 !docker pull node:18     # Descargar una versión específica
 !docker image rm <nombre> # Eliminar una imagen
 !clear                   # Limpiar la terminal
 ```
 
 Estos comandos permiten inspeccionar, descargar y eliminar imágenes según sea necesario.
 %% [markdown]
 ## Crear contenedores
 
 ```bash
 !docker create <nombre_imagen>     # Crea un contenedor desde una imagen
 !docker start <id_contenedor>      # Inicia el contenedor
 !docker ps                         # Lista contenedores en ejecución
 !docker stop <id_contenedor>       # Detiene un contenedor
 !docker ps -a                      # Muestra todos los contenedores
 !docker rm <nombre_contenedor>     # Elimina un contenedor
 ```
 %% [markdown]
 ## Port Mapping
 
 El **port mapping** conecta un puerto del contenedor con uno del host para permitir el acceso externo.  
 
 Ejemplo:  
 - Dos apps NodeJS escuchan en el puerto interno 3000.  
 - En el host se acceden desde los puertos 3000 y 3001 respectivamente.  
 - MongoDB usa el puerto 27017.  
 
 Esto evita conflictos y permite ejecutar varios servicios simultáneamente.
 %% [markdown]
 ## Conexión con contenedores
 
 Una aplicación Node.js puede conectarse a un contenedor MongoDB usando Mongoose.  
 
 ```js
 import express from 'express';
 import mongoose from 'mongoose';
 
 const app = express();
 
 mongoose.connect('mongodb://usuario:password@localhost:27017/miapp?authSource=admin');
 
 app.get('/', (req, res) => res.send('Servidor conectado a MongoDB'));
 
 app.listen(3000, () => console.log('Servidor ejecutándose en puerto 3000'));
 ```
 
 Este código crea un servidor con Express conectado a una base de datos MongoDB.
 %% [markdown]
 ## Nota importante
 
 Para conectarte a contenedores correctamente:
 - Crea una cuenta en [Docker Hub](https://hub.docker.com/).  
 - Configura autenticación según la documentación del contenedor usado.
 %% [markdown]
 ## Ejemplo: creación de un contenedor MongoDB
 
 ```bash
 !docker run -d -p 27017:27017 --name mi_mongo \
   -e MONGO_INITDB_ROOT_USERNAME=usuario \
   -e MONGO_INITDB_ROOT_PASSWORD=contraseña \
   mongo
 ```
 
 Luego verifica:
 ```bash
 !docker ps
 ```
 Y abre el navegador en `localhost:27017` para comprobar la ejecución.
 %% [markdown]
 ## Dockerfile
 
 ```dockerfile
 FROM node:18
 RUN mkdir -p /home/app
 COPY . /home/app
 EXPOSE 3000
 CMD ["node", "/home/app/index.js"]
 ```
 
 Define cómo crear la imagen y qué se ejecutará cuando el contenedor inicie.
 %% [markdown]
 ## Docker Compose
 
 Permite automatizar la creación y conexión de múltiples contenedores.
 
 ```yaml
 version: '3'
 services:
   app:
     build: .
     ports:
       - "3000:3000"
     depends_on:
       - mongo
   mongo:
     image: mongo
     ports:
       - "27017:27017"
     environment:
       MONGO_INITDB_ROOT_USERNAME: usuario
       MONGO_INITDB_ROOT_PASSWORD: password
 ```
 
 Ejecuta todo con:
 ```bash
 !docker-compose up
 ```
 %% [markdown]
 ## Volúmenes
 
 Los volúmenes permiten persistir datos fuera del contenedor.  
 Tipos:
 - **Anónimo:** creado automáticamente sin nombre.  
 - **Nombrado:** tiene un identificador reutilizable.  
 - **De host:** enlaza carpetas del sistema anfitrión con las del contenedor.
 
 Ejemplo:
 ```bash
 !docker run -v /data/db:/var/lib/mongodb mongo
 ```
 %% [markdown]
 ## Reflexión final
 
 Docker simplifica el despliegue y la portabilidad del software.  
 Permite estandarizar entornos, minimizar errores y automatizar tareas de infraestructura.
 
 ---
 
 **Recursos adicionales:**  
 - [Documentación oficial de Docker](https://docs.docker.com/)  
 - [Guía Docker para principiantes – Docker Docs](https://docs.docker.com/get-started/)  
 - [Tutorial Compose – Docker](https://docs.docker.com/compose/gettingstarted/)
