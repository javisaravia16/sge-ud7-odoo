# Errores de la instalación

La instalación no me salió a la primera. Estos son los problemas que tuve de verdad, en el orden en que me fueron saliendo, y cómo los arreglé.

## 1. La página de Odoo no cargaba

**Lo que salía:** al abrir `localhost:8069` el navegador decía «No se puede acceder a este sitio» y `ERR_CONNECTION_REFUSED`.

**Por qué pasaba:** el contenedor de Odoo estaba parado en Docker Desktop, así que no había nada funcionando en ese puerto.

**Cómo lo arreglé:** abrí Docker Desktop, fui a *Containers* y arranqué el contenedor de Odoo y el de PostgreSQL. Esperé unos segundos, recargué y ya salió Odoo.

**Dónde lo miré:** en la ayuda de Docker Desktop sobre cómo arrancar contenedores.

## 2. Entraba en Odoo pero solo veía «Mi cuenta»

**Lo que salía:** podía entrar, pero no aparecían las aplicaciones ni los ajustes, solo una página de «Mi cuenta».

**Por qué pasaba:** en vez de crear la base de datos me había **registrado** desde la web. Eso crea un usuario de tipo portal, que es como un cliente, no un administrador.

**Cómo lo arreglé:** fui a `localhost:8069/web/database/manager` y creé una base de datos nueva y limpia llamada `momomadrona`. Al crearla se entra directamente como administrador.

**Dónde lo miré:** en la documentación de Odoo sobre los tipos de usuario.

## 3. «Access Denied» al crear la base de datos

**Lo que salía:** «Database creation error: Access Denied».

**Por qué pasaba:** para crear una base de datos Odoo pide la contraseña maestra, y la que yo ponía no era la buena. Fui a mirarla al archivo `/etc/odoo/odoo.conf`, pero allí está guardada cifrada (`admin_passwd = $pbkdf2-sha512$…`), así que no se puede leer.

**Cómo lo arreglé:** cambié esa línea del archivo por una contraseña nueva, reinicié el contenedor de Odoo desde Docker Desktop y volví a crear la base de datos con la contraseña nueva.

**Dónde lo miré:** en la documentación de Odoo sobre el gestor de bases de datos y en la página de la imagen de Odoo en Docker Hub.

## 4. «Permission denied» al cambiar el archivo

**Lo que salía:** al intentar cambiar la contraseña con `sed -i` en la terminal del contenedor salía `sed: couldn't open temporary file /etc/odoo/sed…: Permission denied`.

**Por qué pasaba:** la terminal del contenedor entra con el usuario `odoo`. Ese usuario puede cambiar el archivo, pero no crear archivos nuevos en esa carpeta, y `sed -i` necesita crear uno temporal.

**Cómo lo arreglé:** hice el cambio guardándolo primero en `/tmp` y luego copié el resultado encima del archivo original:

```
sed 's/^admin_passwd.*/admin_passwd = MiClaveNueva/' /etc/odoo/odoo.conf > /tmp/odoo.conf && cat /tmp/odoo.conf > /etc/odoo/odoo.conf
```

**Dónde lo miré:** en Stack Overflow, en preguntas sobre este mismo error de `sed`.

## Lo que he aprendido

- Si `localhost:8069` no carga, lo primero es mirar si el contenedor está encendido.
- No es lo mismo registrarse en la web que crear la base de datos.
- La contraseña maestra hay que apuntarla en un sitio seguro, porque luego Odoo la guarda cifrada y no se puede consultar.
