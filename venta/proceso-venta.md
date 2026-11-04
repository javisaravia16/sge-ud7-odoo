# Proceso de venta

Aquí cuento cómo hice una venta completa en Odoo, desde el presupuesto hasta la factura cobrada. Cada paso lo hice entrando con el usuario que le toca por su puesto, no con el administrador.

## La venta

El cliente es **Concept Store El Patio Azul**, una tienda pequeña de Sevilla que quiere vender nuestra ropa. Nos pide:

- 30 camisetas «Madroño» a 24,90 € cada una, con un 10 % de descuento por llevarse tantas.
- 12 sudaderas «Momo» a 49,90 €.
- 10 pantalones de lino «Alameda» a 39,90 €.

Sin IVA son 1.670,10 €. Con el IVA del 21 % (350,72 €) se queda en **2.020,82 €**.

## Paso 1: Marta hace el presupuesto

Entré con **Marta Ruiz**, la comercial. En Ventas → Presupuestos → Nuevo elegí el cliente y añadí las tres líneas. A las camisetas les puse el 10 % en la columna de descuento.

También puse que el presupuesto vale hasta el **19/11/2026** y que el plazo de pago es de **30 días**.

Luego le di a Imprimir y salió el PDF: [presupuesto.pdf](presupuesto.pdf). Odoo le puso el número **S00001** y lo dejó como «Presupuesto enviado».

Para que saliera la columna de descuento antes tuve que activarlo en Ventas → Configuración → Ajustes (con el administrador).

## Paso 2: Javier lo confirma

Entré con **Francisco Javier Saravia**, el responsable de ventas. Él sí ve los presupuestos de todo el equipo, así que vio el de Marta y le dio a **Confirmar**.

El presupuesto S00001 pasó a ser un **pedido de venta**. Como las tres prendas llevan control de stock, Odoo creó solo la orden de entrega.

## Paso 3: Andrés prepara la entrega

Entré con **Andrés Pineda**, el de almacén. En Inventario → Entregas estaba la orden **WH/OUT/00001**.

Comprobé que las cantidades eran las del pedido (30, 12 y 10) y le di a **Validar**. Odoo preguntó si quería mandar un SMS al cliente y le dije que no. La entrega quedó como **Hecho**.

El stock bajó así:

- Camisetas: de 150 a 120.
- Sudaderas: de 60 a 48.
- Pantalones: de 40 a 30.

## Paso 4: Elena hace la factura

Entré con **Elena Gutiérrez**, la de administración. Ella solo ve el menú Ventas → A facturar → Pedidos a facturar. Ahí abrió el S00001 y le dio a **Crear factura** (factura normal).

Revisó que estaban bien el cliente, las líneas, el descuento y el plazo de pago, y le dio a **Confirmar**. La factura quedó publicada como **INV/2026/00001**, con fecha 04/11/2026 y vencimiento 04/12/2026.

Después la imprimió: [factura.pdf](factura.pdf).

## Paso 5: Elena apunta el cobro

En la misma factura le dio a **Registrar pago**, eligió el banco y el importe de 2.020,82 €. Odoo creó el pago **PBNK1/2026/00001** y la factura se puso en **Pagado**.

## Cómo quedó al final

Volví a entrar con Javier y abrí el S00001: ya pone **Totalmente facturado**, con la entrega hecha y la factura pagada. En Ventas → Informes sale la venta de 1.670,10 € (sin IVA) a nombre de Marta.

## Documentos

- [Presupuesto impreso desde Odoo](presupuesto.pdf)
- [Factura impresa desde Odoo](factura.pdf)
