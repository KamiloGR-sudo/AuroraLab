# Estado final del diagnóstico

## Estado del hardware

Resultado: APROBADO

Evidencias:

- El disco fue detectado correctamente por el sistema.
- SMART no reportó sectores reasignados.
- SMART no reportó sectores pendientes.
- SMART no registró errores.
- La prueba SMART Short finalizó con el estado:
  - Completed without error

Conclusión:

El hardware del disco se encuentra en buen estado.

---

## Estado del sistema de archivos

Resultado: FALLA

Evidencias:

- Linux no pudo montar la partición NTFS.
- ntfsfix detectó corrupción en el archivo $UpCase.
- ntfsfix no logró reparar completamente la estructura.
- TestDisk detectó la partición, pero no pudo listar los archivos.
- El montaje manual también falló.

Conclusión:

La corrupción corresponde al sistema de archivos NTFS y no al hardware del disco.

---

## Diagnóstico final

Diagnóstico: Corrupción lógica del sistema de archivos NTFS.

Estado del hardware: Funcional.

Estado de la partición: Existe.

Estado del sistema de archivos: Gravemente corrupto.

Acción recomendada:

Recrear la partición, formatear el disco y realizar pruebas funcionales.

---

# Etapa de recuperación del disco

## Inicio de la recuperación

Después de confirmar que el hardware del disco estaba en buen estado y que el problema correspondía únicamente al sistema de archivos NTFS, se inició el proceso de recuperación.

Como el disco no contenía información que fuera necesario conservar, se decidió eliminar la estructura anterior y crear nuevamente una partición con un sistema de archivos NTFS completamente nuevo.

Objetivo de esta etapa:

- Recuperar el funcionamiento del disco.
- Garantizar la compatibilidad con Windows y Linux.
- Validar lectura y escritura antes de devolver el disco a sus propietarios.

## Creación de la nueva partición

Se utilizó `fdisk` para crear una nueva partición que ocupara todo el espacio disponible del disco.

Durante el proceso, `fdisk` detectó que aún existía una firma NTFS de la estructura anterior y preguntó si debía eliminarse.

Se respondió **Sí**, con el objetivo de eliminar completamente los rastros del sistema de archivos anterior antes de crear la nueva estructura.

Después de confirmar la configuración, se guardó la nueva tabla de particiones en el disco.

## Creación del sistema de archivos NTFS

Una vez creada la partición, se generó un nuevo sistema de archivos NTFS utilizando `mkfs.ntfs`.

El proceso finalizó correctamente y se creó una estructura NTFS completamente nueva sobre la partición.

Resultado obtenido:

- Sistema de archivos NTFS creado correctamente.
- El proceso terminó sin errores.
- El disco quedó listo para realizar pruebas de montaje y escritura.

## Pruebas de funcionamiento en Linux

Se realizaron las siguientes verificaciones:

- Montaje de la partición NTFS.
- Verificación del espacio disponible.
- Creación de un archivo de prueba.
- Comprobación de lectura y escritura.
- Eliminación del archivo de prueba.
- Desmontaje seguro de la unidad.

### Resultado

Las pruebas fueron exitosas.

Se confirmó que el disco funciona correctamente en Linux y quedó listo para iniciar las pruebas de compatibilidad con Windows.