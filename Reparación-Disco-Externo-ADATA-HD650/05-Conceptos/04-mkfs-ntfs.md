# ¿Qué es mkfs.ntfs?

## Definición

`mkfs.ntfs` es una herramienta de Linux utilizada para crear un sistema de archivos NTFS en una partición. Su función es preparar la partición para que pueda almacenar información utilizando el formato NTFS.

---

## ¿Para qué sirve?

Con `mkfs.ntfs` es posible:

- Crear un nuevo sistema de archivos NTFS.
- Preparar una partición para ser utilizada por Windows y Linux.
- Reemplazar un sistema de archivos dañado por uno nuevo.

---

## Importancia en este proyecto

Después de crear una nueva partición con `fdisk`, fue necesario darle un formato para que pudiera almacenar archivos.

Para ello se utilizó el comando:

```bash
sudo mkfs.ntfs -f /dev/sdb1
```

Este proceso creó un nuevo sistema de archivos NTFS, permitiendo que el disco volviera a ser reconocido correctamente por Linux y Windows.

---

## Lo que aprendimos

Crear un sistema de archivos no repara la información anterior; prepara la partición para comenzar desde cero. Por ello, antes de utilizar `mkfs.ntfs`, es importante confirmar que no sea necesario recuperar los datos existentes.

Este proyecto reforzó uno de los principios de AuroraLab:

> **Comprender antes que modificar.**