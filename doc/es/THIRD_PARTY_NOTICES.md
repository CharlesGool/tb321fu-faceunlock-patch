---
name: project-third-party-notices-es
description: Componentes de terceros, licencias y obligaciones de cumplimiento en la distribución
metadata:
  version: "1.0.0"
  lang: "es"
---

# Avisos de terceros

## Multilingüe

[简体中文](../THIRD_PARTY_NOTICES.md) | [English](../en/THIRD_PARTY_NOTICES.md) | **Español**

## Documentación

- Descripción general del proyecto: [README](README.md)

- Justificación del diseño: [DESIGN](DESIGN.md)

- Estado del proyecto: [LOG](LOG.md)
- Registros históricos: [HISTORY](HISTORY.md)
- Historial de cambios: [CHANGELOG](CHANGELOG.md)

- Avisos de terceros: [THIRD_PARTY_NOTICES](THIRD_PARTY_NOTICES.md)

## Avisos de terceros

### Componentes

| Componente | Versión o hash | URL de origen | Licencia | Uso | Titular de los derechos | Obligaciones de distribución | Verificado el |
| --- | --- | --- | --- | --- | --- | --- | --- |
| ParanoidSense | `a500478bbb0e2933276d57796d9b410f0f8e90f8` (rama `fifteen`) | https://github.com/PixelOS-AOSP/packages_apps_ParanoidSense | Apache-2.0 | `parts/002.app/src/app/` es una modificación suya | Paranoid Android, PixelOS | Conservar el aviso de copyright, adjuntar la licencia, indicar las modificaciones | 2026-11-05 |
| SDK de reconocimiento facial de Megvii (4 so como `libmegface.so`, `vendorImplPrebuilt.jar`, `model_file`) | Igual que arriba | Igual que arriba (`lib/`, `res/raw/`) | Desconocida (el upstream no la declara por separado) | No está en el repositorio; se descarga durante la construcción y se incorpora al `FaceUnlock.apk` compilado localmente | Megvii | Sin determinar; **MUST NOT** entrar en el repositorio | 2026-11-05 |
| Recursos de la página educativa de Ajustes (`face_education_lottie.json`, `face_settings.mp4`) | Igual que arriba, SHA-256 `7788f9ec...`, `f93fe186...` | Igual que arriba (`overlay/res/raw/`) | Propietaria (recursos de Ajustes de Google Pixel, el upstream no la declara por separado) | No está en el repositorio; se descarga durante la construcción y se incorpora al `FaceUnlockSettingsOverlay.apk` compilado localmente | Google | **MUST NOT** entrar en el repositorio | 2026-11-05 |
| Parche `0009-feat-Add-Face-Unlock-with-ParanoidSense` de KLC_OS | commit `f78e9bc3` dentro del parche | https://github.com/LoggingNewMemory/KLC_OS | No declarada en el repositorio; los encabezados de los archivos fuente son Apache-2.0 | 15 clases de `parts/001.services-hooks/src/java/` están adaptadas a partir de él | The Android Open Source Project, Paranoid Android | Conservar el aviso de copyright de los encabezados de los archivos fuente | 2026-11-05 |
| AOSP frameworks/base | Android 15, Android 17 | https://android.googlesource.com/platform/frameworks/base | Apache-2.0 | Las clases de `parts/001.services-hooks/src/java/` derivan del código facial de AOSP | The Android Open Source Project | Conservar el aviso de copyright | 2026-11-05 |

### Textos de licencia

Texto completo de la Apache License 2.0: [LICENSE](../../LICENSE).

### Oferta de código fuente

Este repositorio no distribuye archivos binarios. La biblioteca y el modelo de Megvii los ofrece el upstream en forma binaria, sin código fuente.

### Revisión de cumplimiento

Por ahora no hay componentes copyleft. Las condiciones de redistribución del SDK de Megvii y de los recursos de la página educativa de Ajustes no se pueden determinar; este repositorio solo ofrece el script de descarga. Los APK y los paquetes de flasheo que el usuario construya por su cuenta contienen estos archivos, y antes de distribuirlos el usuario **MUST** confirmar por su cuenta la autorización.
