# Configuración de Jenkins con Docker

Ya que vamos a trabajar sin una Máquina Virtual y utilizaremos Docker, he creado este archivo `docker-compose.yml` para levantar un contenedor de Jenkins que tiene acceso a ejecutar comandos Docker (ideal para nuestro Pipeline de CI/CD).

## Instrucciones para levantar Jenkins

1. Abre tu terminal en la carpeta `jenkins-setup`.
2. Ejecuta el comando para levantar el contenedor en segundo plano:
   ```bash
   docker compose up -d
   ```
3. Jenkins estará disponible en tu navegador en `http://localhost:8080`.
4. Para obtener la contraseña inicial de administrador, ejecuta el siguiente comando:
   ```bash
   docker exec -it jenkins cat /var/jenkins_home/secrets/initialAdminPassword
   ```

Con esto podrás saltarte los pasos del PDF sobre la instalación en Linux y pasar directamente a la **sección 7** de la guía (Instalar plugins mínimos).
