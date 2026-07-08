# CI/CD Web Demo con Jenkins y Docker

Este repositorio contiene una demostración práctica de Integración Continua y Despliegue Continuo (CI/CD) automatizada mediante **Jenkins** y **Docker**. El propósito del proyecto es servir como un laboratorio donde cada commit al código desencadena una tubería (pipeline) de validación, prueba y despliegue hacia entornos controlados.

## Arquitectura del Proyecto

El proyecto se compone de una aplicación web estática sencilla que es servida a través de un servidor NGINX. Toda la orquestación está contenida (dockerizada) para mantener consistencia entre los entornos de desarrollo, prueba y producción.

### Componentes Principales
- **Código de Aplicación (`app/index.html`)**: El código fuente de nuestra web.
- **Dockerfile**: La receta para empaquetar la aplicación web dentro de un servidor NGINX ligero (`nginx:alpine`).
- **docker-compose.yml**: Orquesta los dos entornos de ejecución de la aplicación web:
  - **Staging** (Pre-producción) en el puerto `8081`.
  - **Production** (Producción) en el puerto `8082`.
- **Scripts de Prueba (`scripts/test.sh`)**: Simuladores de pruebas unitarias que el pipeline debe verificar antes de cualquier despliegue.

## Arquitectura del CI/CD (Pipeline)

La magia ocurre en el archivo `Jenkinsfile`. Este archivo declara de manera programática el ciclo de vida de nuestra aplicación utilizando un **Jenkins Declarative Pipeline**. 

La tubería consta de los siguientes pasos estrictos:

1. **Checkout**: Jenkins descarga automáticamente la última versión del código desde GitHub tras detectar un nuevo commit.
2. **Lint / Validación**: Verifica que los archivos fundamentales (`Dockerfile`, `docker-compose.yml`, `index.html`, etc.) existan y sean accesibles.
3. **Test**: Ejecuta el script de pruebas automatizadas (`test.sh`). **Si las pruebas fallan, el pipeline se detiene por completo (se marca en rojo)** protegiendo el entorno de despliegue.
4. **Build (Staging)**: Si el código es válido, se construye una imagen Docker de la aplicación con la etiqueta de staging.
5. **Deploy a Staging**: La imagen recién construida se despliega automáticamente en el entorno de pruebas local en el puerto `8081`.
6. **Aprobación Manual**: El pipeline entra en pausa. Jenkins espera que un administrador revise el entorno de Staging y dé un clic manual de **"Aprobar"** antes de llevar el código a producción.
7. **Promoción de Imagen**: Tras la aprobación, la imagen Docker es reetiquetada como *production* para asegurar inmutabilidad (se despliega exactamente lo mismo que se testeó).
8. **Deploy a Producción**: Se despliega de manera definitiva el servicio en el entorno de Producción en el puerto `8082`.

## Configuración del Entorno de Jenkins

Jenkins también se ha automatizado mediante Docker (`jenkins-setup`). Para desplegar el orquestador:

1. Ubícate en el directorio `jenkins-setup/`.
2. Ejecuta `docker compose up -d` para levantar el contenedor de Jenkins.
3. El propio Jenkins utiliza su plugin `docker-workflow` teniendo acceso al motor interno de Docker para poder compilar y levantar los contenedores de la aplicación web.

> **Nota de Seguridad**: Se integraron sesiones dinámicas (CSRF Tokens) y API Tokens para interactuar programáticamente con el servidor de Jenkins sin comprometer la seguridad.

---
*Práctica de CI/CD automatizada construida como caso de estudio.*
