# Expansión de la partición de Linux en Asus K550D

**Estado:** ✅ Finalizado

## Objetivo

Aumentar el espacio disponible de la partición de Linux Mint en un Asus K550D sin reinstalar el sistema operativo ni perder la información existente. Para lograrlo, se redujo el espacio asignado a Windows 10 y posteriormente se amplió la partición de Linux utilizando GParted desde un entorno Live USB.

## Situación inicial

El equipo Asus K550D tenía instalado Windows 10 y Linux Mint en un esquema de arranque dual (Dual Boot).

Con el paso del tiempo, Linux Mint se convirtió en el sistema operativo principal para el desarrollo de proyectos, documentación y aprendizaje, mientras que Windows 10 pasó a utilizarse de forma ocasional.

La partición de Linux contaba con aproximadamente 80 GB, espacio que comenzaba a ser insuficiente para el crecimiento del sistema y de los proyectos almacenados en él.

Por otro lado, la partición de Windows disponía de una cantidad considerable de espacio libre que no estaba siendo aprovechada.

Debido a esta situación, se tomó la decisión de reducir el tamaño de la partición de Windows en 40 GB para transferir ese espacio a Linux Mint, evitando una reinstalación del sistema operativo y conservando toda la información existente.

## Análisis del problema

Después de reducir la partición de Windows 10 en 40 GB, se obtuvo un espacio sin asignar dentro del disco. Sin embargo, dicho espacio no podía utilizarse directamente para ampliar la partición de Linux.

La razón era que el espacio sin asignar quedó ubicado antes de la partición de Linux (`/dev/sda5`). Para que una partición pueda ampliarse con GParted, el espacio libre debe ser contiguo y encontrarse en la posición adecuada.

Por este motivo fue necesario iniciar el equipo desde una memoria USB con Linux Mint (Live USB), ya que la partición del sistema no puede modificarse mientras se encuentra en uso.

Una vez iniciado el entorno Live, se utilizó GParted para mover la partición `/dev/sda5` hacia la izquierda, ocupando el espacio sin asignar. Posteriormente, la partición fue ampliada hasta utilizar la totalidad de los 40 GB liberados desde Windows.

Este procedimiento permitió redistribuir el espacio del SSD sin reinstalar ninguno de los Sistemas Operativos y conservando toda la información almacenada.

## Procedimiento realizado

El procedimiento se dividió en dos etapas principales.

### 1. Reducción de la partición de Windows

La reducción del espacio se realizó desde Windows 10 utilizando la herramienta **Administración de discos** (`diskmgmt.msc`).

Se redujo la partición principal de Windows en **40 GB (40960 MB)**, generando un espacio sin asignar dentro del SSD.

Finalizada esta operación, la distribución del disco quedó compuesta por:

- Partición del sistema EFI.
- Partición reservada de Microsoft (MSR).
- Partición principal de Windows 10.
- 40 GB de espacio sin asignar.
- Partición de Linux Mint (`/dev/sda5`).
- Partición de recuperación de Windows.

Aunque el espacio ya estaba disponible, todavía no era posible ampliar la partición de Linux debido a la posición en la que había quedado el espacio sin asignar.

### 2. Reorganización y ampliación de la partición de Linux

Se creó una memoria USB booteable con Linux Mint y el equipo fue iniciado desde el entorno Live.

Desde GParted se verificó que la partición `/dev/sda5` no estuviera montada, condición necesaria para modificarla de forma segura.

Posteriormente se realizaron las siguientes operaciones:

1. Desplazar la partición `/dev/sda5` hacia la izquierda hasta eliminar el espacio libre existente antes de ella.
2. Ampliar la partición utilizando la totalidad del espacio disponible.
3. Aplicar los cambios desde GParted.
4. Esperar a que finalizara la comprobación del sistema de archivos (`e2fsck`) y el movimiento de la partición.
5. Reiniciar el equipo y comprobar el correcto funcionamiento del sistema operativo.

## Resultado final

La operación se completó correctamente sin presentar errores durante el proceso de redimensionamiento.

Después de reiniciar el equipo, Linux Mint inició con normalidad y el gestor de arranque (GRUB) continuó permitiendo el acceso tanto a Linux como a Windows 10.

La verificación mediante el comando:

```bash
df -h /
```

confirmó que la partición raíz (`/`) había aumentado de aproximadamente **80 GB** a **118 GiB** (equivalente a unos **120 GB**), quedando disponible una mayor capacidad para futuras instalaciones, proyectos y almacenamiento de información.

La redistribución del espacio se realizó sin pérdida de datos y sin necesidad de reinstalar ninguno de los sistemas operativos.

## Aprendizajes

Durante este procedimiento se reforzaron varios conceptos relacionados con la administración de particiones en Linux y Windows.

- La reducción de una partición en Windows no amplía automáticamente una partición de Linux; únicamente genera espacio sin asignar.
- La posición del espacio sin asignar es determinante. Para ampliar una partición, el espacio libre debe ser contiguo a ella.
- Cuando la partición que se desea modificar contiene el sistema operativo en uso, es necesario trabajar desde un entorno Live USB.
- GParted permite mover y redimensionar particiones de forma segura, siempre que se realicen las operaciones correctamente y se tomen las precauciones necesarias.
- Antes de modificar particiones es recomendable contar con una copia de seguridad de la información importante.
- Verificar el resultado final mediante herramientas como `df -h` permite confirmar que los cambios fueron aplicados correctamente.

## Conclusiones

La redistribución del espacio del SSD permitió adaptar el almacenamiento del equipo a las necesidades reales de uso, asignando una mayor capacidad a Linux Mint, que se ha convertido en el sistema operativo principal para el desarrollo de proyectos y el aprendizaje.

El procedimiento demostró que es posible modificar la distribución de las particiones sin reinstalar los sistemas operativos, siempre que se comprendan las limitaciones del esquema de particiones y se utilicen las herramientas adecuadas.

Este caso también puso de manifiesto la importancia de planificar cada paso antes de realizar cambios sobre el almacenamiento, verificando el estado de las particiones y confirmando el resultado final una vez completado el procedimiento.

La documentación de este proceso en AuroraLab permitirá utilizar esta experiencia como referencia en futuras ampliaciones de almacenamiento o trabajos similares en otros equipos.

<p align="center">
  <img src="Resultado Final.jpg" width="400">
</p>


---
**Proyecto finalizado:** ✅ Sí

**Estado de validación:** Probado y verificado en hardware real.