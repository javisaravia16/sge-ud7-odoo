# Usuarios, grupos y permisos

## La idea: cada uno solo lo que necesita

En Momo Madroña trabajan pocas personas, pero cada una tiene un puesto muy claro. He seguido el **principio de mínimo privilegio**: a cada usuario le doy acceso solo a lo que necesita para hacer su trabajo y nada más. Así, si alguien se equivoca, el fallo se queda en su parte y no puede tocar, por ejemplo, la contabilidad o borrar clientes.

> Por seguridad, aquí solo pongo el **correo de acceso** de cada usuario. Las contraseñas no aparecen ni en el repositorio ni en el vídeo.

## Tipos de usuario en Odoo

| Tipo | Qué es | ¿Lo uso? |
|------|--------|----------|
| **Interno** | Trabajadores de la empresa. Entran al «backend» de Odoo y ven las apps según sus permisos. | Sí, para toda la plantilla. |
| **Portal** | Clientes o proveedores. Solo entran a «Mi cuenta» para ver sus presupuestos, pedidos y facturas. No ven nada de dentro. | Sí, para la persona de compras de El Patio Azul. |
| **Público** | El visitante anónimo de la web, sin iniciar sesión. Odoo lo tiene creado por defecto. | No hace falta crear ninguno. |

## Plantilla de usuarios

| Usuario | Correo de acceso | Puesto | Tipo | Ventas | Facturación | Inventario | Otros | Por qué |
|---------|------------------|--------|------|--------|-------------|------------|-------|---------|
| Administrador | admin@momomadrona.es | Responsable de sistemas | Interno | Administrador | Administrador | Administrador | Ajustes | Es la cuenta que monta y configura Odoo. **No se usa para el proceso de venta.** |
| **Francisco Javier Saravia Ogazón** | javier.saravia@momomadrona.es | Responsable de ventas | Interno | Administrador | — | — | — | Tiene que ver los presupuestos de todo el equipo, confirmarlos y sacar informes de ventas. No toca facturas ni almacén. |
| Marta Ruiz Ocaña | marta.ruiz@momomadrona.es | Comercial | Interno | Usuario: solo mostrar documentos propios | — (sin acceso) | — | Grupo **Momo Madroña / Comercial junior** | Solo prepara presupuestos para sus clientes. No debe ver los de otros compañeros ni la contabilidad. |
| Andrés Pineda Lora | andres.pineda@momomadrona.es | Almacén | Interno | — | — | Usuario | — | Prepara y envía los pedidos. Solo necesita Inventario para validar las salidas. |
| Elena Gutiérrez Ríos | elena.gutierrez@momomadrona.es | Administración y facturación | Interno | — (no gestiona ventas) | Facturación | — | Grupo **Momo Madroña / Facturación de pedidos** | Hace las facturas y apunta los cobros. Puede leer los pedidos para facturarlos, pero no crear ni modificar presupuestos. |
| Rocío Almansa Ferrer (El Patio Azul) | compras@elpatioazul.example | Cliente | Portal | — | — | — | — | Así la tienda puede ver y descargar sus presupuestos y facturas sin llamarnos. |

Todos los usuarios internos tienen su avatar o foto, y cada uno pertenece a la compañía *Momo Madroña S.L.*

**Dónde se hace:** Ajustes → Usuarios y compañías → Usuarios → Nuevo. En la pestaña **Permisos de acceso** se elige el nivel de cada app.
**Usuario portal:** Contactos → Rocío Almansa Ferrer → Acción → *Conceder acceso al portal*.

## Grupos propios

Para crearlos activé el **modo desarrollador** (Ajustes → al final de la página → *Activar el modo de desarrollador*) y fui a **Ajustes → Usuarios y compañías → Grupos → Nuevo**.

En los derechos de acceso las letras significan: **L** = leer, **E** = escribir (modificar), **C** = crear, **B** = borrar.

### Momo Madroña / Comercial junior

Asignado a: **Marta Ruiz Ocaña**

- **Grupos heredados:** Ventas / Usuario: solo mostrar documentos propios. Con esto Marta ya tiene todo lo que trae ese nivel y encima se le suma lo de este grupo.
- **Para qué sirve:** dejar claro lo que puede hacer un comercial que acaba de entrar: hacer presupuestos y dar de alta clientes, pero sin poder borrar nada ni cambiar el catálogo ni los precios de las prendas.

| Modelo | Nombre en Odoo | L | E | C | B | Motivo |
|--------|----------------|:-:|:-:|:-:|:-:|--------|
| Contacto | `res.partner` | ✔ | ✔ | ✔ | ✘ | Puede dar de alta y corregir clientes, pero no borrarlos. |
| Plantilla de producto | `product.template` | ✔ | ✘ | ✘ | ✘ | Solo consulta el catálogo; los precios los decide el responsable. |
| Pedido de venta | `sale.order` | ✔ | ✔ | ✔ | ✘ | Crea y edita sus presupuestos, pero no los borra (se cancelan). |

- **Menús:** ninguno añadido. El menú de Contabilidad no le aparece porque no tiene ningún grupo de facturación.

### Momo Madroña / Facturación de pedidos

Asignado a: **Elena Gutiérrez Ríos**

- **Grupos heredados:** Facturación / Facturación.
- **Para qué sirve:** que Elena pueda abrir los pedidos ya confirmados y crear su factura, sin poder hacer presupuestos.

| Modelo | Nombre en Odoo | L | E | C | B | Motivo |
|--------|----------------|:-:|:-:|:-:|:-:|--------|
| Pedido de venta | `sale.order` | ✔ | ✔ | ✘ | ✘ | Lee el pedido y Odoo apunta en él que ya está facturado. No crea pedidos. |
| Línea de pedido | `sale.order.line` | ✔ | ✔ | ✘ | ✘ | Para que se actualice la cantidad facturada de cada línea. |
| Asistente «Crear factura» | `sale.advance.payment.inv` | ✔ | ✔ | ✔ | ✘ | Es la ventanita que sale al pulsar *Crear factura* en el pedido. |

- **Menú añadido:** Ventas → A facturar → *Pedidos a facturar*. Es lo único que ve de Ventas.

## Comprobación de que funciona

Inicié sesión con cada usuario (en una ventana de incógnito) y comprobé esto:

| Usuario | Lo que **sí** puede | Lo que **no** puede |
|---------|---------------------|---------------------|
| Marta (comercial) | Crear presupuestos y clientes. Ver solo **sus** presupuestos. | No ve el menú de Contabilidad/Facturación ni Inventario. Al intentar borrar un cliente sale *«No tiene permiso para eliminar…»*. No ve los presupuestos de Javier. |
| Javier (responsable de ventas) | Ver todos los presupuestos, confirmarlos y ver el análisis de ventas. | No tiene acceso a facturas ni a Inventario. |
| Andrés (almacén) | Ver y validar las órdenes de entrega. Ver el stock. | No ve Ventas ni Facturación. |
| Elena (facturación) | Ver los pedidos a facturar, crear y publicar facturas, registrar pagos. | No puede crear presupuestos ni ve el resto del menú de Ventas. |
| Rocío (portal) | Entrar en «Mi cuenta» y ver el presupuesto y la factura de la tienda. | No ve nada del interior de Odoo. |
