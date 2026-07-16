# Comandos utilizados

> **Importante**
>
> En este proyecto el disco fue identificado como **`/dev/sdb`** en el portátil **Asus K550D** (Linux Mint 22.3 Cinnamon) y como **`/dev/sda`** en el **Lenovo IdeaPad D330 (82H0)** (Linux Mint 22.3 XFCE).
>
> En Linux, el nombre del dispositivo puede cambiar según el orden en que el sistema detecte los discos. Antes de ejecutar cualquier comando, identifique siempre el dispositivo correcto utilizando herramientas como `lsblk`.

Este documento recopila los comandos utilizados durante el diagnóstico, reparación y validación del disco externo ADATA HD650.

---

## Diagnóstico

### TestDisk

**Objetivo**

Analizar la estructura lógica del disco e intentar detectar problemas en las particiones y en el sistema de archivos.

**Comando**

```bash
sudo testdisk
```

**Explicaciones**

Inicia la herramienta TestDisk en modo interactivo para analizar el disco y ofrecer opciones de recuperación o diagnóstico.

**Resultado esperado**

Permite inspeccionar la estructura del disco sin realizar cambios hasta que el usuario confirme una acción.

### ntfsfix -d

**Nivel de dificultad**

🟡 Intermedio

**Objetivo**

Intentar eliminar la marca **dirty** del sistema de archivos NTFS.

**Comando**

```bash
sudo ntfsfix -d /dev/sdX1
```

**Explicación**

La opción `-d` intenta limpiar la marca que indica que Windows considera el sistema de archivos como no cerrado correctamente o pendiente de revisión.

Durante este proyecto se utilizó como parte de las pruebas para mejorar la compatibilidad con Windows.

**Resultado esperado**

Si la estructura NTFS lo permite, la marca es eliminada y Windows deja de solicitar una comprobación del disco.

**Buenas prácticas**

- Ejecutarlo únicamente después de identificar correctamente la partición.
- Comprobar posteriormente el comportamiento del disco en Windows.

**Errores comunes**

- Pensar que este comando repara cualquier daño del sistema de archivos.
- Ejecutarlo sobre una partición incorrecta.

### ntfsfix -b

**Nivel de dificultad**

🔴 Avanzado

**Objetivo**

Intentar reconstruir el sector de arranque NTFS utilizando la copia de respaldo.

**Comando**

```bash
sudo ntfsfix -b /dev/sdX1
```

**Explicación**

La opción `-b` intenta recuperar el sector de arranque NTFS cuando existe una copia válida almacenada por el propio sistema de archivos.

En este proyecto formó parte de las pruebas realizadas para intentar recuperar completamente la estructura NTFS.

**Resultado esperado**

Si la copia de respaldo es válida, el sector de arranque queda restaurado.

**Buenas prácticas**

- Ejecutarlo únicamente cuando exista sospecha de corrupción del sector de arranque.
- Confirmar previamente el estado del disco mediante herramientas de diagnóstico.

**Errores comunes**

- Utilizarlo como primera opción sin realizar un diagnóstico previo.
- Esperar que resuelva daños que no están relacionados con el sector de arranque.

### smartctl

**Nivel de dificultad**

🟡 Intermedio

**Objetivo**

Comprobar el estado físico del disco mediante la tecnología SMART (Self-Monitoring, Analysis and Reporting Technology).

**Comandos**

```bash
sudo smartctl -t short /dev/sdX
```

Inicia una prueba corta del disco.

```bash
sudo smartctl -l selftest /dev/sdX
```

Muestra el resultado de las pruebas realizadas.

**Explicación**

SMART permite conocer el estado de salud del disco duro, detectar sectores defectuosos, errores de lectura, temperatura y otros indicadores que ayudan a determinar si la falla es física o lógica.

En este proyecto, SMART confirmó que el hardware del disco estaba en buen estado, permitiendo enfocar el diagnóstico en el sistema de archivos NTFS.

**Resultado esperado**

La prueba debe finalizar sin errores críticos y proporcionar información suficiente para evaluar el estado físico del disco.

### fdisk

**Nivel de dificultad**

🔴 Avanzado

**Objetivo**

Crear, eliminar y modificar particiones en un disco.

**Comando**

```bash
sudo fdisk /dev/sdX
```

**Explicación**

`fdisk` es una herramienta de administración de particiones para discos con esquema MBR. Durante este proyecto se utilizó para eliminar la partición dañada y crear una nueva partición que ocupara todo el espacio disponible del disco.

Dentro de `fdisk` se utilizaron los siguientes comandos:

- `p` → Mostrar la tabla de particiones.
- `d` → Eliminar una partición.
- `n` → Crear una nueva partición.
- `w` → Guardar los cambios y salir.

**Resultado esperado**

Una nueva tabla de particiones correctamente creada y lista para recibir un nuevo sistema de archivos.

**Buenas prácticas**

- Confirmar el nombre del disco con `lsblk` antes de ejecutar `fdisk`.
- Revisar la tabla de particiones antes de guardar los cambios.
- Mantener una copia de seguridad cuando exista información importante.

**Errores comunes**

- Ejecutar `fdisk` sobre el disco equivocado.
- Guardar cambios sin verificar la partición seleccionada.
- Modificar un disco que contiene información sin tener una copia de seguridad.

---

## Reparación

Dentro de `fdisk` se utilizaron los siguientes comandos:

- `p` → Mostrar la tabla de particiones.
- `d` → Eliminar la partición.
- `n` → Crear una nueva partición.
- `w` → Guardar los cambios.

Posteriormente:

### mkfs.ntfs

**Nivel de dificultad**

🔴 Avanzado

**Objetivo**

Crear un nuevo sistema de archivos NTFS sobre una partición.

**Comando**

```bash
sudo mkfs.ntfs -f /dev/sdX1
```

**Explicación**

`mkfs.ntfs` inicializa una partición con un nuevo sistema de archivos NTFS. En este proyecto se utilizó después de reconstruir la tabla de particiones para reemplazar la estructura lógica dañada por una nueva completamente funcional.

> **Advertencia:** Este comando elimina el sistema de archivos existente en la partición seleccionada. Debe utilizarse únicamente cuando ya no sea necesario conservar la información o exista una copia de seguridad.

**Resultado esperado**

La partición queda preparada para ser montada y utilizada tanto en Linux como en Windows.

**Buenas prácticas**

- Verificar que la partición seleccionada sea la correcta.
- Confirmar que existe una copia de seguridad antes de ejecutar el comando.
- Comprobar posteriormente el funcionamiento montando la partición.

**Errores comunes**

- Ejecutar el comando sobre la partición equivocada.
- Formatear una partición que aún contiene información importante.

---

### lsblk

**Nivel de dificultad**

🟢 Básico

**Objetivo**

Mostrar los discos, particiones y sistemas de archivos detectados por Linux.

**Comando**

```bash
lsblk -f
```

**Explicación**

`lsblk` permite identificar los dispositivos de almacenamiento conectados al equipo, mostrando información como el nombre del dispositivo, el sistema de archivos, la etiqueta, el UUID y el punto de montaje.

En este proyecto fue una herramienta fundamental para confirmar si el disco era `/dev/sda` o `/dev/sdb` según el equipo utilizado.

**Resultado esperado**

Visualizar correctamente la estructura de los dispositivos de almacenamiento y verificar el nombre del disco antes de ejecutar comandos que puedan modificarlo.

**Buenas prácticas**

- Ejecutar `lsblk -f` antes de utilizar herramientas como `fdisk`, `mkfs`, `mount` o `ntfsfix`.
- Confirmar siempre el nombre del dispositivo antes de realizar cambios.

**Errores comunes**

- Asumir que el disco siempre tendrá el mismo nombre (`/dev/sda`, `/dev/sdb`, etc.).
- Ejecutar comandos sobre un dispositivo sin haberlo identificado previamente.

### mkdir

**Nivel de dificultad**

🟢 Básico

**Objetivo**

Crear un directorio que sirva como punto de montaje para acceder al contenido de una partición.

**Comando**

```bash
sudo mkdir /mnt/disco_GNA
```

**Explicación**

`mkdir` crea un nuevo directorio en el sistema de archivos. En este proyecto se utilizó para crear el punto de montaje donde posteriormente se montó la partición NTFS del disco externo.

**Resultado esperado**

Se crea correctamente el directorio `/mnt/disco_GNA`, listo para utilizarse como punto de montaje.

**Buenas prácticas**

- Utilizar nombres descriptivos para los puntos de montaje.
- Verificar que el directorio no exista previamente.
- Mantener organizados los puntos de montaje dentro de `/mnt` o `/media`, según la finalidad.

**Errores comunes**

- Intentar montar una partición sobre un directorio que no existe.
- Crear el directorio en una ubicación inadecuada.

### mount

**Nivel de dificultad**

🟡 Intermedio

**Objetivo**

Montar una partición para que el sistema operativo pueda acceder a su contenido.

**Comando**

```bash
sudo mount /dev/sdX1 /mnt/disco_GNA
```

**Explicación**

`mount` asocia una partición con un punto de montaje del sistema de archivos, permitiendo acceder a los archivos y directorios almacenados en ella.

En este proyecto se utilizó para verificar que la nueva partición NTFS podía montarse correctamente después de la reparación.

**Resultado esperado**

La partición queda accesible desde el directorio `/mnt/disco_GNA` y es posible leer y escribir archivos.

**Buenas prácticas**

- Verificar previamente que exista el punto de montaje.
- Confirmar que la partición no esté montada antes de ejecutar el comando.
- Desmontar la partición de forma segura cuando finalicen las pruebas.

**Errores comunes**

- Intentar montar una partición sobre un directorio inexistente.
- Montar la partición equivocada.
- Desconectar el disco sin desmontarlo previamente.

### df

**Nivel de dificultad**

🟢 Básico

**Objetivo**

Verificar que la partición se encuentre montada y comprobar la capacidad y el espacio disponible del sistema de archivos.

**Comando**

```bash
df -h
```

**Explicación**

`df` muestra el uso del espacio en los sistemas de archivos montados. La opción `-h` presenta la información en un formato más fácil de leer (KB, MB, GB, TB).

En este proyecto se utilizó para confirmar que la partición NTFS había sido montada correctamente y que su capacidad era la esperada.

**Resultado esperado**

La partición aparece en la lista de sistemas de archivos montados con su capacidad total, espacio utilizado, espacio disponible y punto de montaje.

**Buenas prácticas**

- Utilizar la opción `-h` para facilitar la lectura de los datos.
- Verificar que el punto de montaje corresponda a la partición correcta.
- Comparar la capacidad mostrada con la capacidad esperada del disco.

**Errores comunes**

- Interpretar incorrectamente las unidades de almacenamiento.
- Confundir la partición montada con otro sistema de archivos del equipo.

### touch

**Nivel de dificultad**

🟢 Básico

**Objetivo**

Crear un archivo de prueba para verificar que la partición permite escribir información correctamente.

**Comando**

```bash
sudo touch /mnt/disco_GNA/prueba.txt
```

**Explicación**

`touch` crea un archivo vacío si este no existe. En este proyecto se utilizó para comprobar que el disco no solo podía montarse, sino también aceptar la creación de nuevos archivos.

**Resultado esperado**

Se crea el archivo `prueba.txt` dentro de la partición montada, confirmando que el sistema de archivos permite operaciones de escritura.

**Buenas prácticas**

- Utilizar archivos temporales para las pruebas de escritura.
- Eliminar los archivos de prueba una vez finalizadas las verificaciones.
- Comprobar posteriormente que el archivo fue creado correctamente.

**Errores comunes**

- Ejecutar el comando sobre un punto de montaje incorrecto.
- Interpretar un error de permisos como una falla del disco sin verificar primero el montaje.

### ls

**Nivel de dificultad**

🟢 Básico

**Objetivo**

Verificar que el archivo de prueba fue creado correctamente y consultar sus permisos, propietario, tamaño y fecha de modificación.

**Comando**

```bash
ls -l /mnt/disco_GNA
```

**Explicación**

La opción `-l` (long listing) muestra información detallada de los archivos y directorios. En este proyecto se utilizó para confirmar que `prueba.txt` existía realmente dentro del disco reparado.

**Resultado esperado**

Se muestra el archivo `prueba.txt` junto con su información detallada, confirmando que la operación de escritura fue exitosa.

**Buenas prácticas**

- Verificar el contenido del directorio después de crear archivos de prueba.
- Revisar los permisos y el propietario cuando existan problemas de acceso.

**Errores comunes**

- Consultar un directorio diferente al punto de montaje.
- Confundir un directorio vacío con un fallo en la creación del archivo.

### rm

**Nivel de dificultad**

🟢 Básico

**Objetivo**

Eliminar el archivo de prueba creado durante la validación del disco.

**Comando**

```bash
sudo rm /mnt/disco_GNA/prueba.txt
```

**Explicación**

`rm` elimina archivos del sistema. En este proyecto se utilizó para comprobar que, además de crear archivos, el disco permitía eliminarlos correctamente.

**Resultado esperado**

El archivo `prueba.txt` desaparece del directorio, confirmando que la partición permite operaciones de eliminación.

**Buenas prácticas**

- Verificar que se está eliminando el archivo correcto.
- Utilizar archivos de prueba para este tipo de validaciones.
- Revisar posteriormente el contenido del directorio.

**Errores comunes**

- Eliminar un archivo importante por no verificar la ruta.
- Utilizar `rm` sin confirmar qué archivo se eliminará.

### umount

**Nivel de dificultad**

🟡 Intermedio

**Objetivo**

Desmontar la partición de forma segura antes de desconectar el disco.

**Comando**

```bash
sudo umount /mnt/disco_GNA
```

**Explicación**

`umount` finaliza el acceso al sistema de archivos y asegura que todos los datos pendientes hayan sido escritos en el disco antes de retirarlo.

En este proyecto fue el último paso de la validación en Linux.

**Resultado esperado**

La partición deja de aparecer como montada y el disco puede desconectarse de forma segura.

**Buenas prácticas**

- Cerrar cualquier archivo o carpeta abierta del disco antes de desmontarlo.
- Confirmar que el comando se ejecutó correctamente antes de retirar el dispositivo.

**Errores comunes**

- Desconectar el disco sin desmontarlo.
- Intentar desmontar una partición que aún está siendo utilizada por algún programa.