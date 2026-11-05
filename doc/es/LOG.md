---
name: project-log-es
description: Defectos, limitaciones, decisiones y traspaso del proyecto
metadata:
  version: "1.0.0"
  lang: "es"
---

# Registro

## Multilingüe

[简体中文](../LOG.md) | [English](../en/LOG.md) | **Español**

## Documentación

- Descripción general del proyecto: [README](README.md)

- Justificación del diseño: [DESIGN](DESIGN.md)

- Estado del proyecto: [LOG](LOG.md)
- Registros históricos: [HISTORY](HISTORY.md)
- Historial de cambios: [CHANGELOG](CHANGELOG.md)

- Avisos de terceros: [THIRD_PARTY_NOTICES](THIRD_PARTY_NOTICES.md)

## Registros

- [Registros históricos](HISTORY.md)
- [Historial de cambios](CHANGELOG.md)

## Errores

- [ ] La versión bp1a solo se verificó sin conexión (javac, `check_dex`, puntos de inyección localizados, recompilación de smali coherente); no hay resultado registrado en dispositivo real.
- [ ] La versión cp3a solo se verificó sin conexión; la línea de imagen base cp3a (Android 17) entra en bucle de arranque en dispositivo real y todavía no se puede probar en él.
- [ ] La pérdida de efecto de las sobrescrituras en subclases causada por el inlining de R8 no se detecta sin conexión; solo se ve en los registros del dispositivo.

## Limitaciones

- La biometría WEAK (Class 2) solo sirve para desbloquear y para comprobaciones débiles; no puede autorizar pagos ni desbloquear credenciales de Keystore que exijan biometría fuerte.
- Reconocimiento 2D: su resistencia a ataques con fotos y vídeos es inferior a la de la luz estructurada.
- Las 3 partes solo se han verificado habilitadas todas a la vez.

## Decisiones

| Fecha | Decisión | Estado | Motivo |
| --- | --- | --- | --- |
| 2026-10-27 | Autorizar `USE_BIOMETRIC_INTERNAL` según la identidad de priv-app del sistema | Aceptada | Los dos permisos necesarios son de nivel signature y la lista blanca de privapp no puede concederlos |
| 2026-10-27 | SenseService se enlaza con `BIND_FOREGROUND_SERVICE` y `BIND_INCLUDE_CAPABILITIES` | Aceptada | Evita la restricción foreground-only de CAMERA y no necesita notificación permanente |
| 2026-11-01 | Fusionar 3 parches en un solo parche con 3 partes | Aceptada | Funcionalmente ninguno puede faltar, así que elegirlos por separado no tiene sentido |
| 2026-11-05 | Cambiar el nombre del paquete de la app a `com.android.faceunlock` | Aceptada | Evita la detección que toma `co.aospa.sense` como rasgo de una ROM de terceros |
| 2026-11-05 | Un directorio por ID de imagen base (ap3a, bp1a, cp3a) | Aceptada | Las firmas de clases y los puntos de inyección de `services.jar` cambian con la imagen base |
| 2026-11-05 | Separar en un repositorio público independiente, como única fuente del parche, y Mods conserva una copia sincronizada | Aceptada | Coincide con herald-core y tb321fu-tango-patch; el tutorial general se queda en paranoidsense-gsi-port |
| 2026-11-05 | `FaceUnlockSettingsOverlay.apk` deja de subirse al repositorio y los recursos de la página educativa se obtienen del upstream durante la construcción | Aceptada | Ese APK contiene recursos propietarios de Ajustes de Pixel |

## Traspaso

Consulte el traspaso vigente en el [LOG.md en chino simplificado](../LOG.md#交接).
