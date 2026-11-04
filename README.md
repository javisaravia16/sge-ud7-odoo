# SGE UD7 Odoo

Práctica Evaluable 3-7 · Práctica: Odoo de principio a fin — usuarios, privilegios y venta completa (entrega en Git)

Francisco Javier Saravia Ogazón
DAW / DAM — MEDAC Sevilla
Momo Madroña 

Empresa inventada: **Momo Madroña**, una marca de ropa de Sevilla. En esta práctica he instalado Odoo, he configurado la empresa, he creado a los trabajadores con sus permisos y he hecho una venta completa, desde el presupuesto hasta la factura cobrada.

## Índice

1. [Instalación de Odoo](instalacion/) — Odoo 19 Community con Docker
   - [Diapositivas de la instalación (PDF)](instalacion/instalacion-odoo.pdf)
   - [Errores encontrados y cómo los resolví](instalacion/errores-instalacion.md)
2. [La empresa: Momo Madroña](empresa/ficha-empresa.md) — datos, imagen de marca, catálogo y clientes
3. [Usuarios, grupos y permisos](usuarios/usuarios-grupos-permisos.md) — plantilla, grupos propios y pruebas
4. [Proceso de venta](venta/proceso-venta.md) — del presupuesto S00001 a la factura pagada
   - [Presupuesto impreso desde Odoo (PDF)](venta/presupuesto.pdf)
   - [Factura impresa desde Odoo (PDF)](venta/factura.pdf)

## Estructura

```
sge-ud7-odoo/
├── README.md
├── empresa/        → logo y ficha de la empresa
├── instalacion/    → diapositiva de la instalación y errores
├── usuarios/       → usuarios, grupos y permisos
├── venta/          → proceso de venta, presupuesto y factura
└── capturas/       → capturas de apoyo
```
