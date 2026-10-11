# 🐉 3DSwitch-Builder

Builder modular portable para compilar **Atmosphere CFW**, **hekate** y proyectos **devkitPro** desde Windows, con empaquetado automático de la SD y personalización completa de splash screens.

![version](https://img.shields.io/badge/version-9.4.39-blue)
![python](https://img.shields.io/badge/python-3.12-3776AB)
![platform](https://img.shields.io/badge/platform-Windows%2010%2F11-0078D6)

---

## 📋 Tabla de contenidos

- [¿Qué es?](#-qué-es)
- [Características](#-características)
- [Requisitos](#-requisitos)
- [Instalación](#-instalación)
- [Uso](#-uso)
- [Módulos](#-módulos)
- [Personalización de splash](#-personalización-de-splash)
- [Módulos personalizados](#-módulos-personalizados)
- [Arquitectura](#-arquitectura)
- [Solución de problemas](#-solución-de-problemas)
- [Compilar el EXE](#-compilar-el-exe)
- [Desarrollo](#-desarrollo)
- [Licencia](#-licencia)

---

## 🎯 ¿Qué es?

**3DSwitch-Builder** es una aplicación de escritorio que **automatiza por completo** el proceso de compilar un CFW para Nintendo Switch:

1. **Clona** los repositorios oficiales (Atmosphere-NX, CTCaer/hekate).
2. **Configura** devkitPro portable (sin instalación global).
3. **Parchea** el código fuente automáticamente (nombre del CFW, versión, splash).
4. **Compila** con las flags correctas.
5. **Empaqueta** todo en un ZIP listo para copiar a la SD.

Pensado para gente que quiere **customizar su CFW** (nombre, splash, versión) sin pelearse con Makefiles, dependencias, o scripts de build.

---

## ✨ Características

### Compilación

- 🔨 **Atmosphere CFW** con soporte para último tag estable o master.
- 🚀 **hekate (CTCaer)** con bootlogo personalizado y `hekate_ipl.ini` configurable.
- 🛠️ **Proyectos devkitPro libres** (homebrew, emuladores, librerías).
- 🌿 **Auto-clone** de repos si no están presentes.
- 🔄 **Re-clone limpio** si algo se corrompe.
- 🩹 **Pre-build** con parches para scripts Python de terceros.
- 📦 **Instalación automática** de paquetes devkitPro vía pacman.

### Personalización

- 🏷️ **Nombre del CFW** que aparece en `Ajustes → Sistema → Información`.
- 🔢 **Versión forzada** (o dejar la real de Atmosphere).
- 🖼️ **Tres tipos de splash**:
  - `boot_splash_screen_text.inc` (fusee, con texto encima).
  - `boot_splash_screen_notext.inc` (package3, sin texto).
  - `atmosphere/boot.bmp` (splash alternativo, no recompila).
- 🖼️ **Bootlogo de hekate** (`bootloader/res/bootlogo.bmp`).
- ⚙️ **hekate_ipl.ini** con opciones configurables desde la GUI.

### Empaquetado SD

- 💾 **ZIP único** con todos los módulos.
- 📝 **LEEME.txt** con las versiones de cada componente.
- 🔀 **Detección de colisiones** de rutas entre módulos.

### Infraestructura

- 🐍 **Python portable** embebido (no contamina el sistema).
- 🖥️ **devkitPro portable** con MSYS2.
- 🚫 **Sin ventanas negras** de consola al compilar.
- 📝 **Logs completos** con fallback de permisos.
- 🔒 **Licencias** con firma HMAC y binding por equipo.
- 🧩 **Módulos personalizados** por carpeta `custom_modules/`.

### UI

- 🎨 **Tkinter** nativo (sin dependencias pesadas).
- 📑 **Pestañas por módulo** con paneles específicos.
- 🔄 **Thread-safe**: tareas en hilos, UI nunca bloqueada.
- ⏹️ **Cancelación en árbol** (`taskkill /F /T`) para no dejar procesos huérfanos.
- 📊 **Barra de estado con heartbeat** (contador de tiempo).

---

## 📦 Requisitos

### Sistema

- **Windows 10 o 11** (x64).
- **4 GB de RAM** mínimo (8 GB recomendado).
- **20 GB de espacio libre** (devkitPro + workspaces de build).
- Conexión a internet para el primer arranque (clones de repos y paquetes).

### Software

- **Python 3.12** (solo para desarrollo; el EXE va autónomo).
- **devkitPro** puede estar instalado o no — el builder usa su propia copia portable.

---

## 🚀 Instalación

### Desde el EXE (recomendado)

1. Descarga `3DSwitch-Builder.exe` de la sección [Releases](../../releases).
2. Coloca `activation_key.txt` (proporcionado por el autor) en la misma carpeta.
3. Ejecuta `3DSwitch-Builder.exe`.
4. La primera vez descargará devkitPro portable (~500 MB).

### Desde código fuente

```cmd
git clone https://github.com/TU_USUARIO/3DSwitch-Builder.git
cd 3DSwitch-Builder
py -3 -m venv venv
venv\Scripts\activate
pip install -r requirements.txt
py -3 _launcher.py
