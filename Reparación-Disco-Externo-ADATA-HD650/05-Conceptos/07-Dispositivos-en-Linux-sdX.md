# ¿Por qué un mismo disco puede aparecer como /dev/sda o /dev/sdb?

## Definición

En Linux, los dispositivos de almacenamiento se identifican mediante nombres como `/dev/sda`, `/dev/sdb`, `/dev/sdc`, entre otros. Estos nombres son asignados automáticamente por el sistema operativo según el orden en que detecta los dispositivos durante el arranque o al conectarlos.

---

## ¿Para qué sirve esta nomenclatura?

La nomenclatura `sdX` permite a Linux identificar cada dispositivo de almacenamiento de forma única para poder administrarlo correctamente.

Por ejemplo:

- `/dev/sda` → Primer dispositivo detectado.
- `/dev/sdb` → Segundo dispositivo detectado.
- `/dev/sdc` → Tercer dispositivo detectado.

Las particiones de cada dispositivo se identifican agregando un número al final, por ejemplo:

- `/dev/sda1`
- `/dev/sdb1`

---

## Importancia en este proyecto

Durante la recuperación del disco externo ADATA HD650 se observó que el mismo dispositivo recibió nombres diferentes según el equipo utilizado.

En el portátil **Asus K550D** con **Linux Mint 22.3 Cinnamon**, el disco fue identificado como:

```text
/dev/sdb1
```

En el **Lenovo IdeaPad D330** con **Linux Mint 22.3 XFCE**, el mismo disco fue identificado como:

```text
/dev/sda1
```

Esto ocurrió porque Linux asigna estos nombres de acuerdo con el orden en que detecta los dispositivos y no porque el disco haya cambiado.

---

## Lo que aprendimos

No se debe asumir que un mismo disco siempre conservará el mismo nombre (`/dev/sda`, `/dev/sdb`, etc.). Antes de ejecutar comandos que modifiquen un dispositivo, es indispensable verificar su identificación mediante herramientas como `lsblk` o `fdisk -l`.

Este proyecto reforzó uno de los principios de AuroraLab:

> **Comprender antes que modificar.**