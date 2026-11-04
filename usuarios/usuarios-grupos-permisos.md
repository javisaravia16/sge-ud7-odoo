# Usuarios, grupos y permisos

## La idea

En Momo Madroña cada persona tiene un puesto distinto, así que he intentado que cada una pueda hacer **solo lo que necesita para su trabajo** (esto se llama principio de mínimo privilegio). Así, si alguien se equivoca, no puede estropear cosas de otra parte, como la contabilidad, ni borrar clientes.

> Aquí solo pongo el correo con el que entra cada usuario. Las contraseñas no están en el repositorio.

## Tipos de usuario que hay en Odoo

- **Interno:** los trabajadores de la empresa. Entran en Odoo y ven las aplicaciones según los permisos que les des. Todos los trabajadores son de este tipo.
- **Portal:** los clientes o proveedores. Solo ven «Mi cuenta», con sus presupuestos, pedidos y facturas. No ven nada de dentro de Odoo. Le he dado este acceso a Rocío, de El Patio Azul.
- **Público:** cualquiera que visite la web sin iniciar sesión. Odoo ya lo tiene creado y no hay que tocar nada.

## Los usuarios

Todos los trabajadores tienen su foto de perfil (un avatar con sus iniciales) y pertenecen a la compañía Momo Madroña S.L. Se crean en **Ajustes → Usuarios y compañías → Usuarios**, y los permisos se eligen en la pestaña **Permisos de acceso**.

### Administrador (admin@momomadrona.es)

Es la cuenta del responsable de sistemas: la que instala y configura Odoo. Es administrador de todo, pero **no la he usado para hacer la venta**.

### Francisco Javier Saravia Ogazón (javier.saravia@momomadrona.es)

Es el **responsable de ventas** y es el usuario con mi nombre. Es interno y en Ventas tiene el permiso **Administrador**, así que ve los presupuestos de todo el equipo, los puede confirmar y ve los informes. No tiene acceso a Facturación ni a Inventario, porque no lo necesita.

### Marta Ruiz Ocaña (marta.ruiz@momomadrona.es)

Es la **comercial**. Es interna y en Ventas tiene **Usuario: solo mostrar documentos propios**, así que solo ve sus presupuestos, no los de los demás. No tiene acceso a Facturación ni a Inventario. Además le he puesto el grupo propio **Momo Madroña / Comercial junior** (lo explico abajo).

### Andrés Pineda Lora (andres.pineda@momomadrona.es)

Es el de **almacén**. Es interno y solo tiene Inventario con el permiso **Usuario**, que es lo que necesita para preparar y validar las entregas. No tiene Ventas ni Facturación.

### Elena Gutiérrez Ríos (elena.gutierrez@momomadrona.es)

Es la de **administración y facturación**. Es interna y en Facturación tiene el permiso **Facturación**, con el que puede hacer facturas y apuntar los cobros. No gestiona ventas: no puede crear presupuestos. Para que pueda facturar los pedidos le he puesto el grupo propio **Momo Madroña / Facturación de pedidos**.

### Rocío Almansa Ferrer (compras@elpatioazul.example)

Es la persona de compras de **El Patio Azul**, que es un cliente. Su usuario es de tipo **portal**, para que la tienda pueda ver sus presupuestos y facturas sin tener que llamarnos. Se lo di desde **Contactos → Rocío → Acción → Conceder acceso al portal**.

## Grupos propios

Para crear grupos hay que activar antes el **modo desarrollador** (al final de la página de Ajustes). Después se crean en **Ajustes → Usuarios y compañías → Grupos → Nuevo**.

En los permisos sobre cada parte de Odoo (Odoo los llama *modelos*) hay cuatro casillas: **L** es leer, **E** es escribir (modificar), **C** es crear y **B** es borrar.

### Momo Madroña / Comercial junior

Lo tiene **Marta**. Sirve para un comercial que acaba de empezar: puede hacer presupuestos y dar de alta clientes, pero no puede borrar nada ni cambiar los precios.

**Hereda** el grupo *Ventas / Usuario: solo mostrar documentos propios*, así que tiene todo lo de ese grupo más lo que le añado aquí:

- **Contactos** (`res.partner`): L sí, E sí, C sí, B no. Puede crear y corregir clientes, pero no borrarlos.
- **Productos** (`product.template`): L sí, E no, C no, B no. Solo puede mirar el catálogo; los precios los pone el responsable.
- **Pedidos de venta** (`sale.order`): L sí, E sí, C sí, B no. Puede hacer y cambiar sus presupuestos, pero no borrarlos.

No le he añadido menús. El menú de Facturación no le sale porque no tiene ningún permiso de facturación.

### Momo Madroña / Facturación de pedidos

Lo tiene **Elena**. Sirve para que pueda abrir los pedidos ya confirmados y hacerles la factura, sin poder crear presupuestos.

**Hereda** el grupo *Facturación / Facturación*, y además:

- **Pedidos de venta** (`sale.order`): L sí, E sí, C no, B no. Puede ver el pedido, y Odoo apunta en él que ya está facturado.
- **Líneas de pedido** (`sale.order.line`): L sí, E sí, C no, B no. Para que se actualice lo facturado en cada línea.
- **Ventana «Crear factura»** (`sale.advance.payment.inv`): L sí, E sí, C sí, B no. Es la ventanita que sale al darle a *Crear factura* en el pedido.

Le he añadido el menú **Ventas → A facturar → Pedidos a facturar**, que es lo único de Ventas que ve.

## Comprobación de que funciona

Entré con cada usuario para probar que los permisos funcionan:

- **Marta** creó el presupuesto y pudo dar de alta un cliente. Cuando intenté borrar un cliente salió el aviso «No puede borrar registros 'Contacto'». Cuando intenté cambiar el precio de una camiseta salió que no puede modificar productos.
- **Javier** vio el presupuesto de Marta (aunque no lo creó él) y lo confirmó.
- **Andrés** validó la entrega en Inventario.
- **Elena** hizo la factura desde el pedido y apuntó el cobro. Cuando intenté crear un presupuesto con ella salió «No puede crear registros 'Pedido de venta'».
- **Rocío** solo ve «Mi cuenta» con los documentos de su tienda.

Todo el detalle de la venta está en [proceso-venta.md](../venta/proceso-venta.md).
