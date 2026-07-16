# ¿Qué es fdisk?

## Definición

`fdisk` es una herramienta de Linux que permite crear, eliminar, modificar y consultar particiones en dispositivos de almacenamiento. Trabaja directamente sobre la tabla de particiones del disco.

---

## ¿Para qué sirve?

Con `fdisk` es posible:

- Visualizar la tabla de particiones.
- Crear nuevas particiones.
- Eliminar particiones existentes.
- Modificar el tipo de una partición.
- Guardar los cambios realizados en el disco.

---

## Importancia en este proyecto

`fdisk` fue la herramienta principal utilizada para reconstruir la estructura lógica del disco externo ADATA HD650.

Con ella se eliminó la partición dañada, se creó una nueva partición y posteriormente se preparó el disco para crear un nuevo sistema de archivos NTFS.

---

## Lo que aprendimos

`fdisk` es una herramienta muy potente, pero también requiere precaución. Un cambio incorrecto puede provocar la pérdida de la información almacenada en un disco.

Este proyecto demostró la importancia de comprender la estructura de las particiones antes de realizar cualquier modificación.

Este proyecto reforzó uno de los principios de AuroraLab:

> **Comprender antes que modificar.**