# Manual

## Objetivo

Restablecer el funcionamiento normal de la interfaz de Windows Search cuando el buscador funciona correctamente, pero su interfaz gráfica aparece corrupta o desconfigurada.

## Síntomas

- La barra de búsqueda abre correctamente.
- Es posible escribir y realizar búsquedas.
- La interfaz gráfica se muestra incompleta, desplazada o corrupta.
- El menú Inicio funciona con normalidad.

## Procedimiento

### 1. Reiniciar el Explorador de Windows

Reiniciar el proceso **Windows Explorer** desde el Administrador de tareas.

---

### 2. Reiniciar los procesos de búsqueda

Finalizar los procesos relacionados con Windows Search para que Windows los inicie nuevamente.

---

### 3. Verificar la integridad del sistema

Ejecutar:

```powershell
sfc /scannow
```

Si no se detectan errores, continuar con el siguiente paso.

---

### 4. Reparar la imagen del sistema

Ejecutar:

```powershell
DISM /Online /Cleanup-Image /RestoreHealth
```

Esperar a que el proceso finalice completamente.

---

### 5. Registrar nuevamente Windows Search

Ejecutar:

```powershell
Get-AppxPackage -AllUsers Microsoft.Windows.Search | Foreach {Add-AppxPackage -DisableDevelopmentMode -Register "$($_.InstallLocation)\AppXManifest.xml"}
```

---

### 6. Reiniciar el equipo

Realizar un reinicio completo del sistema y verificar el funcionamiento de Windows Search.

## Resultado esperado

La interfaz de Windows Search vuelve a mostrarse correctamente sin afectar los archivos personales ni requerir la reinstalación del sistema operativo.