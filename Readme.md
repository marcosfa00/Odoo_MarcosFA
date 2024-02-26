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

## MODIFICACIONES OPEN ACADEMY

```Path
.
├── Readme.md
├── docker-compose.yml
└── extra-addons
    └── openacademy
        ├── __init__.py
        ├── __manifest__.py
        ├── __pycache__
        │   └── __init__.cpython-39.pyc
        ├── controllers
        │   ├── __init__.py
        │   ├── __pycache__
        │   │   ├── __init__.cpython-39.pyc
        │   │   └── controllers.cpython-39.pyc
        │   └── controllers.py
        ├── data
        │   └── datos.xml
        ├── demo
        │   └── demo.xml
        ├── models
        │   ├── __init__.py
        │   ├── __pycache__
        │   │   ├── __init__.cpython-39.pyc
        │   │   └── tabla_nombres.cpython-39.pyc
        │   └── tabla_nombres.py
        ├── security
        │   └── ir.model.access.csv
        └── views
            ├── templates.xml
            └── views.xml


```

Bien, este es nuestro arbol de Directorios, Aquí hemos modificado los siguientes directorios para que una vez creada nuestra tabla en la base de datos Postgres
se pueda mostrar en **odoo**.

### MODELS
La carpeta Models contiene archivos Python que definen la estructura de datos y la lógica de negocios de tu aplicación. Aquí se especifican los modelos de Odoo, que son clases de Python que representan las tablas de la base de datos y proporcionan la lógica para interactuar con esos datos.
Bien, el primer paso para crear una tabla en Postgres dentro de Open academy, será crear un archivo.py dentro del directorio Models
En este caso le hemos lalamdo **tabla_nombres**


```Python
    from odoo import fields, models

class TestModel(models.Model):
    _name = "test_model"
    _description = "Test Model"

    name = fields.Char(String ="Nombre")
    description = fields.Text(String="Descripcion")
```

La siguiente Linea falla, ya que no tenemso odoo instalado en Local, esta dentro del Docker, pero al lanzar el Docker podremso comprobar que funcionará correctaemnte.

          from odoo import fields, models

## Views
La carpeta Views contiene archivos XML que definen la interfaz de usuario. Estos archivos describen cómo se presentan los datos y las acciones en la interfaz de usuario, incluyendo formularios, listas y otras vistas.


Dentro del Directorio Views deberemos descomentar los siguientes apartados
    
        <!-- explicit list view definition -->
         <!-- actions opening views on models -->
           <!-- Top menu item -->
         <!-- menu categories -->
DEl siguiente apartado solo se descomentará la primera parte

         <!-- actions -->

## Security
La carpeta Security se utiliza para definir los modelos de seguridad y los permisos de acceso. En los archivos XML de esta carpeta, puedes especificar quién tiene acceso a ciertas funciones y datos en tu aplicación Odoo.


Bin, tambien vamso a trabajar con Security, aquí editaremos el archivo **id.model.access.csv** para que se vea de la siguiente manera:

id,name,model_id:id,group_id:id,perm_read,perm_write,perm_create,perm_unlink
access_openacademy_openacademy,openacademy.openacademy,model_test_model,base.group_user,1,1,1,1



# FUNCIONAMIENTO

A continuación se mostrarán ciertas imágenes de como hemos creado una tabla con los valores FERNANDO ALONSO Y 33 :

QUE PREVIAMENTE HEMSO DEFINIDO en la carpeta **DATA** en el archivo xml datos:

```XML
<odoo>
    <data>
     <record model="test_model" id="openacademy.nombres">
         <field name="name">FERNANDO ALONSO</field>
         <field name="description">33</field>
     </record>
    </data>
</odoo>

```

# IMAGENES

PODEMOS COMPROBAR COMO EFECTIVAMENTE OPENACADEMY ESTA INSTAALDO, Y TRAS TODAS SUS MODIFICACIOENS SE ENCUENTRA EN LA VERSION 0.7

![OPENACADEMY INSTALADO](Pictures%2FIMAGEN1.png)

TRAS ACTUALIZAR OPENACADEMY PODEMOS COMPORBAR COMO ESTE MODULO (AL HACER UN DOCKER COMPOSE RESTART) YA NOS APARECE EN EL MENU DE APLICACIONES

![IMAGEN2.png](Pictures%2FIMAGEN2.png)

FINALMENTE VEMOS COMO EFECTIVAMENTE APARECE LA FILA CON FERNANDO ALONSO CON DESCRIPCION 33 QUE DEFINIMOS EN DATOS.XML
![IMAGEN3.png](Pictures%2FIMAGEN3.png)

---
MARCOS FDEZ. AVEDNAÑO

