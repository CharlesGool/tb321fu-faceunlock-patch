# tb321fu-faceunlock-patch

<!-- Descomenta la siguiente línea cuando el proyecto tenga un icono -->
<!-- <img align="right" src="../resources/logo.png" width="160" alt="tb321fu-faceunlock-patch"> -->

[简体中文](../../README.md) | [English](../en/README.md) | **Español**

La Legion Y700 de tercera generación (TB321FU) no tiene desbloqueo facial tras flashear la GSI de Pixel Tablet; este proyecto es un parche de TB321FU-GSI-Mods que integra el desbloqueo facial de ParanoidSense en la imagen system, sin necesitar root ni firma de plataforma.

[![License](https://img.shields.io/badge/License-Apache--2.0-orange)](../../LICENSE)

## Documentación

- Descripción general del proyecto: [README](README.md)

- Justificación del diseño: [DESIGN](DESIGN.md)

- Estado del proyecto: [LOG](LOG.md)
- Registros históricos: [HISTORY](HISTORY.md)
- Historial de cambios: [CHANGELOG](CHANGELOG.md)

- Avisos de terceros: [THIRD_PARTY_NOTICES](THIRD_PARTY_NOTICES.md)

## Introducción

- Reconocimiento facial 2D con la cámara frontal, usando el SDK de Megvii, nivel de biometría de Android WEAK (Class 2). El nombre del paquete de la app es `com.android.faceunlock`.
- El parche `faceunlock` consta de 3 partes: el framework Sense y la autorización de permisos en `services.jar`, la app de rostro con su archivo de permisos, y el overlay de la entrada en Ajustes.
- La modificación de `services.jar` depende de la imagen base, por lo que hay un directorio por imagen base y solo se ejecuta cuando el ID de la imagen base coincide:

| Directorio | Imagen base | Estado |
| --- | --- | --- |
| [patch/tangorpro_ap3a.241005.015](../../patch/tangorpro_ap3a.241005.015/README.md) | Pixel Tablet Mystic GSI, Android 15 (AP3A.241005.015) | Verificada en dispositivo real |
| [patch/tangorpro_bp1a.250505.005](../../patch/tangorpro_bp1a.250505.005/README.md) | GSI oficial de Pixel Tablet, Android 15 (BP1A.250505.005) | Verificada sin conexión; resultado en dispositivo real sin registrar |
| [patch/tangorpro_cp3a.260905.009](../../patch/tangorpro_cp3a.260905.009/README.md) | GSI de Pixel Tablet, Android 17 (CP3A.260905.009) | Verificada sin conexión; esta línea de imagen base aún no arranca |

El método de portado independiente de un dispositivo concreto y las herramientas generales (módulos KernelSU / Magisk) están en [paranoidsense-gsi-port](https://github.com/CharlesGool/paranoidsense-gsi-port); este repositorio es su implementación de referencia en la TB321FU.

Este repositorio es la única fuente de este parche. `patches/required/tangorpro/<imagen base>/faceunlock/` de [TB321FU-GSI-Mods](https://github.com/CharlesGool/TB321FU-GSI-Mods) es una copia sincronizada.

## Requisitos

- Legion Y700 de tercera generación (TB321FU), con una de las imágenes base de la tabla anterior
- Construir la imagen system con [TB321FU-GSI-Mods](https://github.com/CharlesGool/TB321FU-GSI-Mods)
- Máquina de construcción: Linux, JDK 17, python3, git, unzip, debugfs (e2fsprogs), y además:
  - [dex2jar](https://github.com/ThexXTURBOXx/dex2jar) 2.4.38, r8 8.5.35 (incluye d8), [Apktool](https://github.com/iBotPeaches/Apktool) 2.12.1
  - Android build-tools 35 (aapt2, zipalign, apksigner), kotlinc 2.0.21
- Red: antes de construir los APK hay que descargar de [PixelOS ParanoidSense](https://github.com/PixelOS-AOSP/packages_apps_ParanoidSense) las bibliotecas de Megvii, los modelos y los recursos de la página educativa de Ajustes. Estos archivos y los APK que los contienen **MUST NOT** subirse al repositorio

## Instalación

Este proyecto no se instala por separado; TB321FU-GSI-Mods lo integra en la imagen system durante la construcción.

### Instalación rápida

TB321FU-GSI-Mods ya incluye una copia sincronizada. Primero compile localmente los dos APK (el ejemplo usa ap3a; para otras imágenes base cambie el directorio) y después construya:

```bash
cd <TB321FU-GSI-Mods>/patches/required/tangorpro/tangorpro_ap3a.241005.015/faceunlock/parts
bash 002.app/src/fetch_upstream.sh
bash 002.app/src/build.sh <directorio system de la imagen base> 002.app/files
bash 003.settings-entry/src/build.sh <directorio system de la imagen base> 003.settings-entry/files
cd <TB321FU-GSI-Mods>
make 9008 NOTE="faceunlock"
```

`services.jar` no hay que compilarlo de antemano; se genera en el momento de la construcción a partir de la imagen base actual.

### Instalación normal

Clone este repositorio junto a Mods (mismo directorio padre), sincronice la copia en Mods y después compile los APK y construya como en la instalación rápida. Para otra ubicación, use `FACEUNLOCK_PATCH_DIR`:

```bash
cd <TB321FU-GSI-Mods>
make sync-faceunlock
```

La sincronización no borra los APK compilados localmente, el upstream descargado ni la clave de firma.

## Guía

### Parámetros

Los parámetros se definen en `devices/<dispositivo>/conf.d/faceunlock.conf` de Mods; una variable de entorno con el mismo nombre tiene prioridad. La descripción de las variables también está en [.env.example](../../.env.example).

| Parámetro | Usado en | Significado |
| --- | --- | --- |
| `APKTOOL_JAR` | services-hooks | jar de Apktool 2.12.1 (parámetro común de Mods) |
| `FU_D2J` | services-hooks | `d2j-dex2jar.sh` de dex2jar 2.4.38 |
| `FU_R8` | services-hooks | jar de r8 8.5.35; con otra versión la función es la misma, pero los bytes del dex cambian |
| `FU_JAVA_HOME` | services-hooks | JDK 17, por defecto `/usr/lib/jvm/java-17-openjdk-amd64` |
| `D2J`, `R8`, `APKTOOL`, `BT`, `KOTLIN_HOME` | compilar APK | Las mismas herramientas de arriba, el directorio de build-tools 35 y el directorio de kotlinc 2.0.21 |
| `FACEUNLOCK_KEYSTORE` | compilar APK | keystore de firma (alias `faceunlock`, contraseña `android`); si no se define, se genera automáticamente |
| `PARANOIDSENSE_DIR` | compilar overlay | Directorio del upstream ParanoidSense, por defecto `002.app/src/third_party/ParanoidSense` |

### Uso y comprobación

Ajustes → Seguridad y privacidad → Desbloqueo del dispositivo → Desbloqueo facial → introducir el PIN → registrar el rostro siguiendo las indicaciones. Comprobación:

```bash
adb shell getprop ro.face.sense_service            # true
adb shell dumpsys face                             # aparece SenseProvider
adb shell cmd overlay list | grep -i faceunlock    # [x]
```

El método para localizar los puntos al cambiar de imagen base está en `parts/001.services-hooks/README.md` de cada directorio de imagen base.

## Actualización

Tras cambiar de imagen base (incluida una versión nueva de la misma línea), basta con construir de nuevo; `services.jar` se genera en el momento a partir de la nueva imagen base. Si cambia el ID de la imagen base hay que crear el directorio correspondiente; para la modificación, consulte el tutorial de portado de [paranoidsense-gsi-port](https://github.com/CharlesGool/paranoidsense-gsi-port). Los datos faciales ya registrados se guardan en los datos de la app y se conservan mientras no se borren.

## Desinstalación

### Desinstalación rápida

Elimine el modelo facial en Ajustes. El parche sigue en la imagen, pero el desbloqueo facial deja de usarse.

### Desinstalación completa

Primero elimine el modelo facial en Ajustes; después quite este parche en Mods, construya de nuevo y flashee:

```bash
make 9008 WITHOUT=faceunlock NOTE="sin rostro"
```

## Agradecimientos

- [PixelOS-AOSP/packages_apps_ParanoidSense](https://github.com/PixelOS-AOSP/packages_apps_ParanoidSense): código fuente de la app, biblioteca y modelo de Megvii
- Parche `0009-feat-Add-Face-Unlock-with-ParanoidSense` de [KLC_OS](https://github.com/LoggingNewMemory/KLC_OS): referencia para la parte del framework

## Licencia

Este proyecto se publica bajo la Apache License 2.0, identificador SPDX `Apache-2.0`; el texto completo está en [LICENSE](../../LICENSE). La biblioteca y el modelo de Megvii, así como los recursos de la página educativa de Ajustes, no se distribuyen con este proyecto; véase [THIRD_PARTY_NOTICES](THIRD_PARTY_NOTICES.md).
