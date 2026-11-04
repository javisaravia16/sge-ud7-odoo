# Errores de la instalación

La instalación con Docker fue bastante limpia, pero me encontré con un par de cosas y, además, provoqué a propósito el problema típico del puerto 8069 ocupado para ver cómo se soluciona.

## Comprobaciones que hice antes de empezar

- Que el ordenador tuviera la virtualización activada (Administrador de tareas → Rendimiento → CPU → «Virtualización: habilitada»).
- Que WSL 2 estuviera instalado (`wsl --status`).
- Que Docker Desktop arrancara y que `docker --version` respondiera.
- Que nada estuviera usando ya el puerto 8069 (`netstat -ano | findstr :8069` no devolvía nada).

## Tabla de errores

| # | Error o mensaje | Causa | Solución aplicada | Fuente consultada |
|---|-----------------|-------|-------------------|-------------------|
| 1 | Docker Desktop: *«WSL 2 installation is incomplete»* | Docker en Windows necesita WSL 2 y no estaba instalado del todo. | Abrí PowerShell como administrador, ejecuté `wsl --install`, reinicié el ordenador y volví a abrir Docker Desktop. | Documentación de Docker Desktop para Windows y de Microsoft sobre WSL |
| 2 | *«Bind for 0.0.0.0:8069 failed: port is already allocated»* (provocado a propósito) | Levanté otro contenedor de Odoo que ya usaba el puerto 8069, así que el segundo no podía usarlo. | Con `docker ps` vi qué contenedor tenía el puerto, lo paré con `docker stop` y volví a lanzar `docker compose up -d`. Otra opción es cambiar el puerto en el compose (`"8070:8069"`) y entrar por `localhost:8070`. | Documentación de Docker Compose (apartado *ports*) |
| 3 | Odoo en el navegador: *«Database connection failure»* nada más arrancar | Odoo arrancó antes de que PostgreSQL estuviera listo para recibir conexiones. | Esperé unos segundos y recargué la página. Para que no pase, el compose tiene `depends_on` y `restart: unless-stopped`, así que Odoo se reintenta solo. | Página de la imagen oficial de Odoo en Docker Hub |
| 4 | *«Internal Server Error»* al recargar justo después de instalar Inventario | La página se recargó mientras Odoo todavía estaba terminando de instalar el módulo. | Esperé a que acabara, reinicié el contenedor con `docker compose restart odoo` y volví a entrar. Ya cargaba bien. | Foro de Odoo (preguntas sobre «Internal Server Error» tras instalar módulos) |

## Lo que he aprendido

- Antes de culpar a Odoo hay que mirar los registros: `docker compose logs -f odoo` dice casi siempre qué está pasando.
- El puerto 8069 solo lo puede usar un programa a la vez.
- Como los datos se guardan en volúmenes, aunque pare o borre los contenedores no se pierde la empresa.
