# 🐉 3DSwitch-Builder

> **Compilador portable y modular de CFW para Nintendo Switch.**
> Todo en uno: Atmosphere, hekate, homebrew y empaquetado SD.

[![Version](https://img.shields.io/badge/version-9.4.34-blue.svg)](https://github.com/)
[![Platform](https://img.shields.io/badge/platform-Windows%2010%2B-lightgrey.svg)](https://github.com/)
[![Python](https://img.shields.io/badge/python-3.12-yellow.svg)](https://www.python.org/)
[![License](https://img.shields.io/badge/license-Private-red.svg)](https://github.com/)

---

## 📋 Índice

- [¿Qué es?](#-qué-es)
- [Características](#-características)
- [Requisitos](#-requisitos)
- [Instalación](#-instalación)
- [Uso rápido](#-uso-rápido)
- [Módulos](#-módulos)
- [Personalización de Atmosphere](#-personalización-de-atmosphere)
- [Sistema de licencias](#-sistema-de-licencias)
- [Arquitectura interna](#-arquitectura-interna)
- [Compilación desde código fuente](#-compilación-desde-código-fuente)
- [Solución de problemas](#-solución-de-problemas)
- [FAQ](#-faq)
- [Contacto](#-contacto)

---

## 🎮 ¿Qué es?

**3DSwitch-Builder** es una aplicación de escritorio para Windows que permite compilar
y empaquetar todo el software necesario para ejecutar Custom Firmware (CFW) en una
Nintendo Switch, **sin necesidad de instalar nada de forma global** en el sistema.

El builder se encarga de:

1. **Clonar** los repositorios oficiales (Atmosphere, hekate, homebrew).
2. **Compilar** cada módulo con la toolchain de devkitPro.
3. **Personalizar** los binarios (nombre del CFW, splash de arranque, config).
4. **Empaquetar** todo en un `.zip` listo para copiar a la SD.

Todo funciona **offline** y **portable**: se puede llevar en un pendrive y ejecutar
en cualquier PC con Windows sin dejar rastro.

---

## ✨ Características

### 🎯 Compilación modular

- **Atmosphere CFW** — el custom firmware más usado, con personalización de
  nombre, versión y splash de arranque.
- **hekate (CTCaer)** — bootloader con editor de `hekate_ipl.ini` y bootlogo.
- **Proyecto devkitPro libre** — cualquier homebrew con `Makefile` estándar de libnx.

### 🔧 Personalización

- **Nombre del CFW** que aparece en *Ajustes → Sistema → Información*.
  Ejemplo: `AMS 1.12.0` → `n3dswitch 1.12.0`.
- **Versión forzada** — puedes sobrescribir la versión que muestra el firmware.
- **Splash de fusee** (imagen + texto renderizado encima).
- **Splash de package3** (imagen sin texto).
- **Splash alternativo** `atmosphere/boot.bmp` (no requiere recompilar).
- **Bootlogo de hekate** `bootloader/res/bootlogo.bmp`.
- **Editor completo de `hekate_ipl.ini`** con todos los toggles.

### 🚀 Robusto y autorreparable

- **Auto-clone**: pulsar "Compilar" sin haber clonado funciona.
- **Build limpio garantizado**: cada compilación parte de cero sin residuos.
- **Cancelación con `taskkill /F /T`**: mata el árbol completo de procesos
  (make, gcc, ld) sin dejar huérfanos.
- **Log detallado** con fallback a `%LOCALAPPDATA%` si el directorio del EXE no
  es escribible.
- **Heartbeat visual** durante builds largos (contador `MM:SS` en la barra de estado).

### 🔐 Sistema de licencias

- Claves firmadas con **HMAC-SHA256** + **nonce aleatorio** de 4 bytes.
- Modos:
  - **HWID genérica** (embebida en `activation_key.txt`).
  - **Ligada a máquina** (Request Code único del equipo).
  - **Permanente** (sin caducidad).
- **Renovación mensual** sin bloqueos artificiales.
- **Anti-debug** y **verificación de integridad** del EXE.

### 📦 Empaquetado inteligente

- Combina aportaciones de todos los módulos en un único `.zip`.
- Añade **Homebrew Loader** (`hbl.nsp`) y **hbmenu** (`hbmenu.nro`) automáticamente.
- Genera **`LEEME.txt`** con versiones y fecha de cada componente.
- Detecta y evita colisiones de rutas entre módulos.

---

## 💻 Requisitos

| Requisito | Mínimo | Recomendado |
|-----------|--------|-------------|
| **Windows** | 10 (64 bits) | 11 |
| **Espacio en disco** | ~2 GB | ~5 GB (para builds paralelos) |
| **RAM** | 4 GB | 8 GB |
| **Conexión a Internet** | Solo la primera vez | Para actualizar repos |

**No requiere** tener Python, devkitPro ni MSYS2 instalados globalmente.
El builder los trae en su propia carpeta `runtime/`.

---

## 📥 Instalación

### Opción A: Ejecutable (recomendado)

1. Descarga el ZIP más reciente desde [Releases](https://github.com/.../releases).
2. **Extrae el contenido completo** en una carpeta (ej. `C:\3DSwitch-Builder\`).
   > ⚠️ **No lo ejecutes desde dentro del ZIP.** Windows bloqueará las
   > escrituras en `runtime/` y el builder fallará.
3. Ejecuta `3DSwitch-Builder.exe`.
4. En el primer arranque se activa automáticamente con la clave embebida
   (`activation_key.txt`).

### Opción B: Desde código fuente

```cmd
git clone https://github.com/tu-usuario/3DSwitch-Builder.git
cd 3DSwitch-Builder
py -3 main.py
```

Consulta la sección [Compilación desde código fuente](#-compilación-desde-código-fuente)
para más detalles.

---

## 🚀 Uso rápido

### 1. Compilar Atmosphere

1. Ve a la pestaña **🐉 Atmosphere CFW**.
2. Pulsa **🔽 Clonar/Actualizar** (o directamente **🔨 Compilar** — clonará solo).
3. Opcionalmente, personaliza:
   - **Nombre del CFW**: `n3dswitch`, `DOW`, `MyCFW`, etc.
   - **Versión forzada**: `1.12.0-4`, `v9`, etc. (déjalo vacío para la real).
   - **Splash de fusee**: imagen 1280×720 (o fondo negro + texto).
   - **Splash de package3**: imagen 1280×720.
4. Pulsa **🔨 Compilar**. Tarda entre **40 y 90 minutos** la primera vez.
5. Cuando termine, verás en el log: `Built package3!` + `✅ PAQUETE SD GENERADO`.

### 2. Empaquetar la SD

1. Ve a la pestaña **💾 Empaquetar SD**.
2. Marca los módulos a incluir.
3. Pulsa **🚀 Compilar TODO y empaquetar** o **💾 Empaquetar SD ahora**.
4. El ZIP final queda en `output/3DSwitch-Builder-SD-YYYYMMDD_HHMMSS.zip`.

### 3. Instalar en la Switch

1. Formatea la SD (exFAT o FAT32).
2. Descomprime el ZIP en la **raíz** de la SD.
3. Inyecta el payload con **TegraRcmGUI** (Windows) o **Rekado** (Android).
4. En el menú de hekate, selecciona **Atmosphere CFW**.

---

## 🧩 Módulos

### 🐉 Atmosphere CFW

| Característica | Descripción |
|----------------|-------------|
| Repo | `Atmosphere-NX/Atmosphere` |
| Estrategia | Tag estable (recomendado) o master |
| Personalización | Nombre, versión, splash, bootlogo |
| Salida | `atmosphere/package3`, `fusee.bin`, `stratosphere.romfs`, etc. |

### 🚀 hekate (CTCaer)

| Característica | Descripción |
|----------------|-------------|
| Repo | `CTCaer/hekate` |
| Estrategia | Master o tag estable |
| Personalización | Bootlogo, `hekate_ipl.ini` completo |
| Salida | `bootloader/`, `payload.bin` |

### 🛠 Proyecto devkitPro libre

| Característica | Descripción |
|----------------|-------------|
| Repo | Cualquiera con `Makefile` de libnx |
| Ejemplos | Homebrew, ports, emuladores |
| Salida | `.nro`, `.nsp`, o lo que defina el Makefile |
| Soporte | hactool (detectado automáticamente) |

---

## 🎨 Personalización de Atmosphere

### Nombre del CFW

Aparece en **Ajustes → Sistema → Información**:

```
Antes:  AMS 1.12.0 (E)
Ahora:  n3dswitch 1.12.0 (E)
```

⚠️ **Requiere recompilar Atmosphere** (no basta con empaquetar la SD).

### Versión forzada

Opcional. Por defecto se usa la versión real del repo. Si la defines, se
sobrescribe el string. **Usa cadenas cortas** — el buffer de Atmosphere
solo admite ~15 caracteres:

| Valor | Resultado |
|-------|-----------|
| (vacío) | `n3dswitch 1.12.0` |
| `v9` | `n3dswitch v9` |
| `1.12.0-4` | `n3dswitch 1.12.0-4` ✅ |
| `1.12.0-4-g0e9cd964c-dirty` | `n3dswitch 1.12.0-4-g0` ❌ (truncado) |

### Splash de fusee

Es la imagen del dragón que se ve **al lanzar el payload** (fase fusee).
Se embebe en `boot_splash_screen_text.inc` como BGRA sin texto.
El builder renderiza encima el nombre + versión.

**Recomendaciones**:
- Tamaño: 1280×720.
- Formato: PNG con transparencia (se convierte a fondo negro).
- Si no das imagen, se genera fondo negro + texto blanco centrado.

### Splash de package3

Es la imagen que se ve **cuando Atmosphere ya está cargando** (después de
fusee). Se embebe en `boot_splash_screen_notext.inc` sin texto.

### Splash alternativo `atmosphere/boot.bmp`

Se aplica **al empaquetar**, sin recompilar. Útil como fallback si los
`boot_splash_screen_*.inc` no se aplican por alguna razón.

---

## 🔐 Sistema de licencias

### Tipos de clave

| Tipo | Duración | Vinculación | Uso |
|------|----------|-------------|-----|
| **HWID** | Cualquiera | Ninguna | `activation_key.txt` embebido |
| **Machine** | Cualquiera | Request Code del equipo | Distribución individual |
| **Permanent** | ∞ | Como arriba | Cliente VIP |

### Cómo se generan

Con `keygen.py` (solo para el autor):

```cmd
:: Clave HWID de 30 días
py -3 keygen.py --cli --days 30 --output activation_key.txt

:: Clave ligada a máquina
py -3 keygen.py --cli --days 30 --req UGZMHVHF6YDRQKJ2 --output cliente.txt

:: Clave permanente
py -3 keygen.py --cli --days 0 --output permanente.txt
```

### Renovación

Cada renovación genera una **clave nueva** (gracias al nonce aleatorio de
4 bytes). No hay bloqueos por reuso. El usuario solo tiene que pegar la nueva
clave en el diálogo de activación.

---

## 🏗 Arquitectura interna

```
3DSwitch-Builder/
├── _launcher.py           # Entry point del EXE + patch global de Popen
├── main.py                # Entry point para desarrollo
├── app_ui.py              # Interfaz Tkinter (ventana, paneles, heartbeat)
├── config.py              # Constantes, rutas, Config thread-safe
├── security.py            # HWID, HMAC, anti-debug, integridad
├── security_license.py    # LicenseManager, BurnState, ActivationDialog
├── build_env.py           # PythonPortable, MSYS, shims, run_bash/run_dkp_bash
├── modules.py             # Atmosphere, hekate, devkitPro módulos
├── sd_packager.py         # Empaquetado final de la SD
├── image_utils.py         # Conversión a BMP 24bpp bottom-up
├── hactool_helper.py      # Wrapper de hactool para proyectos que lo usan
├── keygen.py              # Generador de claves (uso privado del autor)
├── pkg3_patch.py          # Inyección de splash en package3
├── pkg3_diagnose.py       # Análisis del package3
├── clean_pacman_dupes.py  # Utilidad standalone: limpieza de DB pacman
├── fix_pacman_db.py       # Utilidad standalone: reparación de .corrupt
├── setup_cython.py        # Compilación de security.py a .pyd
├── build_complete.bat     # Build + distribución (autor)
├── test_popen.py          # Test del parche de Popen
├── test_renewal.py.py     # Test del nonce en claves
├── logo.ico               # Icono de la app
├── deps.txt               # Dependencias Python (pip install)
├── runtime/               # Python portable + devkitPro + MSYS2 (NO está en git)
├── workspace/             # Repos clonados (se crea al vuelo)
├── output/                # ZIPs finales de la SD
├── deps/                  # Wheels offline (.whl)
└── error.log              # Log de diagnóstico
```

### Flujo de build

```
Usuario pulsa "Compilar"
    │
    ▼
_ensure_repo_cloned()  ──►  si no hay .git, git clone automático
    │
    ▼
_restore_git_sources()  ──►  git checkout -- . (revierte parches previos)
    │
    ▼
_clean_artifacts()  ──►  borra out/ y build/ (nunca romfs ni sources)
    │
    ▼
_apply_cfw_string_patch()  ──►  parchea setsys_mitm_service.cpp
    │
    ▼
_regenerate_splash_incs()  ──►  regenera boot_splash_screen_*.inc
    │
    ▼
make -j1 (haze)  ──►  evita el bug de order-only prerequisites
    │
    ▼
make -j2 (todo)  ──►  build completo (~1h)
    │
    ▼
ZIP de Atmosphere  ──►  listo para empaquetar en SD
```

---

## 🛠 Compilación desde código fuente

### Requisitos de desarrollo

- Python 3.12 (el builder usa el mismo que trae en `runtime/`).
- `pip install pyinstaller` (para generar el EXE).
- Windows 10+ (no soporta Linux/macOS por el uso intensivo de MSYS2/Windows API).

### Ejecutar en modo script

```cmd
py -3 main.py
```

Verás la ventana principal con los tres módulos y la barra de logs.

### Generar el EXE distribuible

```cmd
build_complete.bat
```

El script `build_complete.bat`:

1. Comprueba que existen todos los archivos necesarios.
2. Instala PyInstaller si falta.
3. Compila `_launcher.py` con `--onedir --windowed`.
4. Copia el `runtime/` completo.
5. Elimina `__pycache__` (reduce el tamaño del ZIP).
6. Firma el EXE con HMAC (genera `signature.dat`).
7. Genera una clave embebida de 10 días (`activation_key.txt`).
8. Empaqueta todo en `dist_final/3DSwitch-Builder-v9.4.34.zip`.

### Tests

```cmd
:: Verifica el patch de Popen (CREATE_NO_WINDOW)
py -3 test_popen.py

:: Verifica el nonce de las claves (3 generaciones → 3 claves distintas)
py -3 test_renewal.py.py
```

---

## 🐛 Solución de problemas

### El build falla con `No hay ninguna regla para construir ... .dksh`

**Causa**: bug conocido de Atmosphere con `-j2` y order-only prerequisites
en `troposphere/haze`.

**Solución**: el builder ya lo evita precompilando haze con `make -j1` antes
del `make -j2` principal. Si aún así falla, revisa que la versión de tu
`modules.py` sea ≥ 9.4.30.

### El nombre del CFW no cambia en Ajustes

**Causa**: probablemente solo empaquetaste la SD sin recompilar.

**Solución**: pulsa **🔨 Compilar** en Atmosphere, no solo "Empaquetar SD".
El cambio de nombre requiere recompilar el sysmodule `ams_mitm`.

### La versión no cambia (solo el nombre)

**Causa**: `_apply_cfw_string_patch()` en versiones antiguas solo cambiaba
el nombre, no la versión.

**Solución**: asegúrate de que `modules.py` sea ≥ 9.4.31 y recompila.

### El build se queda colgado sin output

**Causa**: `make` puede tardar minutos en silencio (linkeando).

**Solución**: mira la barra de estado inferior. Aparecerá un contador
`⏳ Compilando Atmosphere CFW — 05:23` que se actualiza cada 5 s. Si sigue
subiendo, el build está vivo.

### No puedo borrar `workspace/` (permisos)

**Causa**: procesos `make`/`gcc` residuales bloquean los handles.

**Solución** (CMD admin):

```cmd
taskkill /F /IM make.exe /T 2>nul
taskkill /F /IM bash.exe /T 2>nul
takeown /F "workspace" /R /D S
icacls "workspace" /grant "%USERNAME%":F /T /C
rmdir /S /Q "workspace"
```

### Aparece "🔒 Depurador activo" al abrir el EXE

**Causa**: falso positivo del anti-debug (solo se activa en modo `.exe`).

**Solución**: abre `security.py`, localiza `is_debugger_present()` y reduce
el umbral `detected >= 2` a `detected >= 3`. Recompila con el `.bat`.

### `signature.dat` no coincide tras recompilar

**Causa**: el hash del EXE cambia cada vez que recompilas.

**Solución**: vuelve a ejecutar `build_complete.bat`. El script genera
`signature.dat` **después** de copiar el EXE a su sitio final, así que
siempre es correcto.

---

## ❓ FAQ

**¿Funciona en Linux o macOS?**
No. Usa MSYS2, `taskkill`, `winreg` y otras APIs exclusivas de Windows.

**¿Necesito instalar devkitPro?**
No. El builder lo trae en `runtime/devkitPro/` y lo usa en modo portable.

**¿Cuánto tarda un build completo?**
- Atmosphere: 40-90 minutos (desde cero).
- hekate: 2-5 minutos.
- Proyecto libre: depende del proyecto.

**¿Cuánto ocupa?**
- ZIP distribuible: ~200 MB (con devkitPro completo).
- Descomprimido: ~2 GB.
- Con repos clonados y builds: hasta 8-10 GB.

**¿Puedo compilar varios proyectos a la vez?**
No directamente (los botones se bloquean durante un build), pero puedes
tener varias instancias del builder en carpetas distintas.

**¿Cómo actualizo el builder?**
Descarga el nuevo ZIP y extrae **sobre la carpeta anterior**, manteniendo
`runtime/` y `workspace/` para no re-clonar ni reinstalar dependencias.

**¿Cómo funciona la renovación de licencia?**
Cuando caduca, aparece el diálogo de activación con tu Request Code. Envías
el código al autor, el autor genera una clave nueva, la pegas, y sigues
30 días más. Repetible indefinidamente.

---

## 📞 Contacto

| Canal | Enlace |
|-------|--------|
| **TikTok** | [@n3dswitch](https://tiktok.com/@n3dswitch) |
| **GitHub** | [Issues](https://github.com/tu-usuario/3DSwitch-Builder/issues) |
| **Email** | (opcional) |

---

## ⚖️ Licencia y descargo de responsabilidad

**Uso privado.** Este software se distribuye con licencia de activación
individual. Está **prohibida su redistribución** sin autorización del autor.

**Descargo de responsabilidad**: Este software se proporciona "tal cual",
sin garantía de ningún tipo. El autor no se responsabiliza de posibles daños
derivados del uso (bricks, pérdida de datos, etc.). El usuario asume toda la
responsabilidad al compilar y ejecutar CFW en su consola.

**Marcas registradas**: "Nintendo", "Nintendo Switch" son marcas registradas
de Nintendo Co., Ltd. Este proyecto no está afiliado, patrocinado ni
respaldado por Nintendo.

---

## 🙏 Agradecimientos

- **Atmosphere-NX** — el CFW.
- **CTCaer** — por hekate.
- **devkitPro** — por la toolchain completa.
- **switchbrew** — por Homebrew Loader y hbmenu.

---

<p align="center">
  <strong>Hecho con ❤️ por n3dswitch</strong><br>
  <em>Última actualización: v9.4.34</em>
</p>
