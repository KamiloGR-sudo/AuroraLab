# Commands

Este documento recopila los comandos utilizados durante el proceso de diagnóstico y reparación del problema presentado en Windows Search.

---

## Verificación de integridad de archivos del sistema

```powershell
sfc /scannow
```

**Resultado:**

La herramienta no encontró infracciones de integridad en los archivos del sistema.

---

## Reparación de la imagen de Windows

```powershell
DISM /Online /Cleanup-Image /RestoreHealth
```

**Resultado:**

La imagen del sistema fue reparada correctamente sin errores.

---

## Registro del componente Windows Search

```powershell
Get-AppxPackage -AllUsers Microsoft.Windows.Search | Foreach {Add-AppxPackage -DisableDevelopmentMode -Register "$($_.InstallLocation)\AppXManifest.xml"}
```

**Resultado:**

El componente Windows Search fue registrado nuevamente sin presentar errores.

---

## Observación

Después de ejecutar los procedimientos anteriores y realizar un reinicio completo del equipo, la interfaz de Windows Search volvió a funcionar correctamente.