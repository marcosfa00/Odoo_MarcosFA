Para levantar tu servicio de Odoo con OpenAcademy y la configuración proporcionada, puedes seguir los siguientes pasos:

1. **Preparación del entorno:**
   - Asegúrate de tener Docker y Docker Compose instalados en tu sistema.

2. **Estructura del Proyecto:**
   - Asegúrate de que tu proyecto tenga la siguiente estructura:
     ```
     - tu_proyecto/
       - addons/
         - openacademy/
           - __init__.py
           - __manifest__.py
           - models/
             - __init__.py
             - tu_modelo.py
           - views/
             - tu_vista.xml
       - docker-compose.yml
     ```

3. **Configuración del archivo `docker-compose.yml`:**
   - Reemplaza el contenido actual de tu archivo `docker-compose.yml` con el siguiente:
     ```yaml
     version: '3.1'
     services:
       web:
         image: odoo:16.0
         depends_on:
           - mydb
         volumes:
           - ./addons:/mnt/extra-addons
         ports:
           - "8069:8069"
         environment:
           - HOST=mydb
           - USER=odoo
           - PASSWORD=myodoo
       mydb:
         image: postgres:15
         environment:
           - POSTGRES_DB=postgres
           - POSTGRES_PASSWORD=myodoo
           - POSTGRES_USER=odoo
         ports:
           - "5433:5432"
     ```

4. **Levantar los Contenedores:**
   - En la terminal, navega hasta la carpeta que contiene tu archivo `docker-compose.yml` y ejecuta el siguiente comando:
     ```bash
     docker-compose up -d
     ```

5. **Instalación del Módulo OpenAcademy en Odoo:**
   - Accede a tu instancia de Odoo a través del navegador, generalmente en `http://localhost:8069`.
   - Ve a "Configuración técnica" > "Actualización de módulos" > "Actualizar la lista de módulos".
   - Busca "openacademy" y selecciona el módulo "OpenAcademy" para instalarlo.

6. **Reinicia el Servicio de Odoo:**
   - Después de instalar el módulo, reinicia el servicio de Odoo ejecutando:
     ```bash
     docker-compose restart web
     ```

Ahora deberías tener tu servicio de Odoo con el módulo OpenAcademy instalado y listo para ser utilizado. ¡Vamos a por la 33!