# Reparación de Disco Externo ADATA HD650

## Información general

**Estado del proyecto:** En desarrollo

**Objetivo:**
Diagnosticar, reparar y validar el funcionamiento de un disco externo ADATA HD650 de 1 TB, documentando todo el proceso de forma técnica y didáctica para futuras consultas.

---

# Fase 1 - Diagnóstico

## Objetivo de la fase

Determinar si la falla del disco externo ADATA HD650 de 1 TB corresponde a un problema físico (hardware) o a un problema lógico (sistema de archivos), utilizando herramientas de diagnóstico en Linux Mint sin modificar inicialmente la información almacenada en el disco.

## Equipos utilizados

### Equipo 1 - Diagnóstico y reparación inicial

- **Portátil:** Asus K550D
- **Procesador:** AMD A10-5750M
- **Sistema operativo:** Linux Mint 22.3 Cinnamon

### Equipo 2 - Validación adicional

- **Portátil:** Lenovo IdeaPad D330 (82H0)
- **Procesador:** Intel Celeron N4020
- **Sistema operativo:** Linux Mint 22.3 XFCE

### Equipo 3 - Validación en Windows

- **Portátil:** Acer Aspire V5-123-3698 (ZHL)
- **Procesador:** AMD E1-2100
- **Sistema operativo:** Windows 10

## Dispositivo en estudio

- **Marca:** ADATA
- **Modelo:** HD650
- **Capacidad:** 1 TB
- **Tipo:** Disco duro externo USB
- **Sistema de archivos detectado:** NTFS
- **Etiqueta de la partición:** GNA

## Observaciones iniciales

Al conectar el disco externo ADATA HD650 al portátil se realizaron las siguientes observaciones antes de ejecutar cualquier herramienta de diagnóstico:

1. El LED indicador del disco encendió de color azul y permaneció estable.

2. Linux Mint detectó inmediatamente el dispositivo USB.

3. El sistema intentó montar automáticamente la partición, pero mostró un mensaje de error indicando que no fue posible montar el volumen.

4. En esta etapa no se realizó ninguna modificación sobre el disco. El objetivo inicial fue recopilar información y realizar un diagnóstico sin poner en riesgo la información almacenada.

---

# Fase 2 - Recuperación del disco

## Objetivo de la fase

Recuperar el funcionamiento del disco externo ADATA HD650 mediante la creación de una nueva partición y un nuevo sistema de archivos NTFS, verificando posteriormente su correcto funcionamiento tanto en Linux como en Windows.

## Condición inicial

Después de finalizar la fase de diagnóstico se confirmó que el hardware del disco se encontraba en buen estado, pero el sistema de archivos NTFS presentaba una corrupción que impedía su utilización.

Debido a que el disco ya no contenía información que fuera necesario conservar, se decidió reconstruir completamente la estructura lógica del disco para devolverlo a un estado funcional.

## Estrategia de recuperación

Antes de realizar cualquier modificación sobre el disco se definió una estrategia de trabajo con el fin de reducir riesgos y comprender la causa real de la falla.

La estrategia consistió en las siguientes etapas:

1. Confirmar si el hardware del disco presentaba fallas físicas mediante herramientas SMART.

2. Verificar el estado del sistema de archivos y de la tabla de particiones utilizando herramientas de diagnóstico.

3. Intentar una recuperación no destructiva siempre que existiera información importante por conservar.

4. Confirmar con el propietario del disco si existía una copia de seguridad antes de realizar modificaciones irreversibles.

5. Reconstruir completamente la estructura lógica del disco únicamente después de confirmar que existía un respaldo de la información.

6. Validar el resultado final tanto en Linux como en Windows para asegurar la compatibilidad del disco recuperado.

## Proceso de recuperación

La recuperación del disco se realizó siguiendo una secuencia controlada de procedimientos. Cada procedimiento tuvo un objetivo específico y fue validado antes de continuar con el siguiente.

### Procedimiento 1 - Reconstrucción de la tabla de particiones

Una vez confirmado que el hardware del disco se encontraba en buen estado y que existía una copia de seguridad de la información, se procedió a reconstruir la tabla de particiones.

Para ello se utilizó la herramienta `fdisk`, eliminando la partición dañada y creando una nueva partición primaria que ocupara todo el espacio disponible del disco.

Los comandos utilizados durante este procedimiento se encuentran documentados en la carpeta **03-Comandos**, donde se explica la función de cada uno y su propósito dentro de la recuperación.

Al finalizar este procedimiento se obtuvo una nueva tabla de particiones lista para crear un nuevo sistema de archivos.

### ¿Por qué se utilizó `fdisk`?

Se utilizó porque permite crear, eliminar y modificar particiones de forma segura desde la terminal de Linux. Era la herramienta adecuada para reconstruir la estructura lógica del disco antes de crear un nuevo sistema de archivos.

Al finalizar este procedimiento se obtuvo una nueva tabla de particiones lista para crear un nuevo sistema de archivos.

### Procedimiento 2 - Creación del sistema de archivos NTFS

Con la nueva partición creada, el siguiente paso consistió en generar un nuevo sistema de archivos NTFS.

Para ello se utilizó la herramienta `mkfs.ntfs`, la cual inicializó la partición con una estructura NTFS completamente nueva.

Al finalizar este procedimiento el disco quedó listo para ser montado y utilizado nuevamente por el sistema operativo.

Los comandos utilizados durante este procedimiento se encuentran documentados en la carpeta **03-Comandos**, donde se explica detalladamente la creación del sistema de archivos NTFS.

### ¿Por qué se utilizó `mkfs.ntfs`?

Se utilizó porque permite crear un nuevo sistema de archivos NTFS desde cero, reemplazando la estructura lógica dañada por una completamente funcional y compatible con Linux y Windows.

### Procedimiento 3 - Validación del funcionamiento

Una vez creado el nuevo sistema de archivos, se verificó que el disco funcionara correctamente antes de dar por finalizada la recuperación.

La validación se realizó en diferentes equipos y sistemas operativos con el objetivo de confirmar que la reparación era compatible en distintos entornos.

Las pruebas realizadas incluyeron:

- Montaje del disco en Linux.
- Verificación de lectura de archivos.
- Verificación de escritura de archivos.
- Eliminación de archivos de prueba.
- Desmontaje seguro del disco.
- Validación del funcionamiento en Windows.
- Comprobación del sistema de archivos mediante las herramientas de Windows.

El resultado confirmó que el disco quedó completamente funcional tanto en Linux Mint como en Windows.

Los comandos utilizados durante las pruebas de validación se encuentran documentados en la carpeta **03-Comandos**, incluyendo las verificaciones realizadas tanto en Linux como durante la comprobación final en Windows.

### ¿Por qué se realizaron pruebas en dos sistemas operativos?

El objetivo fue comprobar que la reparación no solo funcionara en Linux Mint, donde se realizó la recuperación, sino también en Windows, ya que el disco sería utilizado en ambos entornos. La validación confirmó que el sistema de archivos NTFS era completamente funcional en los dos sistemas operativos.

## Conclusiones de la fase

La reconstrucción del sistema de archivos permitió recuperar completamente la funcionalidad del disco externo.

Las pruebas realizadas en Linux Mint y Windows confirmaron que el disco puede utilizarse con normalidad para operaciones de lectura y escritura.

No se detectaron fallas físicas en el hardware durante el proceso de diagnóstico, por lo que la causa del problema se atribuyó a una corrupción lógica del sistema de archivos NTFS.

## Estado de la fase

**Estado:** ✅ Finalizada

**Resultado:**
Disco completamente funcional y validado en Linux y Windows.

**Próxima fase:**
Documentación final del proyecto y publicación en AuroraLab.