# ¿Qué es NTFS?

## Definición

NTFS (New Technology File System) es un sistema de archivos desarrollado por Microsoft para almacenar, organizar y administrar la información en dispositivos de almacenamiento. Es el sistema de archivos utilizado por defecto en las versiones modernas de Windows.

---

## ¿Para qué sirve?

NTFS permite:

- Almacenar archivos y carpetas de forma organizada.
- Gestionar permisos de acceso.
- Soportar archivos y particiones de gran tamaño.
- Mejorar la seguridad e integridad de los datos.

---

## Importancia en este proyecto

El disco externo ADATA HD650 utilizaba el sistema de archivos NTFS. La corrupción de este sistema de archivos impedía que Linux y Windows pudieran acceder correctamente a la información.

Después de reconstruir la partición, se creó nuevamente un sistema de archivos NTFS utilizando la herramienta `mkfs.ntfs`, devolviendo el funcionamiento normal del disco.

---

## Lo que aprendimos

Que un sistema de archivos puede corromperse aunque el disco físico continúe funcionando correctamente. Antes de considerar un daño de hardware, es recomendable verificar el estado del sistema de archivos y realizar un diagnóstico adecuado.

Este proyecto reforzó uno de los principios de AuroraLab:

> **Comprender antes que modificar.**