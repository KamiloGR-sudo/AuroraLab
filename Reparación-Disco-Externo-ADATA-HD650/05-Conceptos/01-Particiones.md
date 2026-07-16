# ¿Qué es una partición?

## Definición

Una partición es una división lógica de un dispositivo de almacenamiento físico, como un disco duro o una unidad SSD. Cada partición funciona de manera independiente y puede contener su propio sistema de archivos, permitiendo al sistema operativo organizar y administrar mejor la información.

---

## ¿Para qué sirve una partición?

Las particiones permiten:

- Organizar la información en un mismo disco.
- Instalar uno o varios sistemas operativos.
- Separar los archivos del sistema de los archivos personales.
- Formatear una partición sin afectar las demás.

---

## Importancia en este proyecto

Durante la recuperación del disco externo ADATA HD650 se comprobó que el problema no estaba relacionado con el hardware, sino con la estructura lógica del disco.

Para recuperar su funcionamiento fue necesario eliminar la partición dañada y crear una nueva utilizando la herramienta `fdisk`. Posteriormente se creó un nuevo sistema de archivos NTFS sobre esa partición.

---

## Lo que aprendimos

Una partición puede dañarse sin que el disco duro presente fallas físicas. Por esta razón, antes de reemplazar un disco es importante realizar un diagnóstico que permita determinar si el problema corresponde al hardware o únicamente a la estructura lógica del almacenamiento.

Este proyecto reforzó uno de los principios de AuroraLab:

> **Comprender antes que modificar.**