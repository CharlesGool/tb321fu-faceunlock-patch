---
name: project-history-es
description: Registros históricos
metadata:
  version: "1.0.0"
  lang: "es"
---

# Registros históricos

## Multilingüe

[简体中文](../HISTORY.md) | [English](../en/HISTORY.md) | **Español**

## Documentación

- Descripción general del proyecto: [README](README.md)

- Justificación del diseño: [DESIGN](DESIGN.md)

- Estado del proyecto: [LOG](LOG.md)
- Registros históricos: [HISTORY](HISTORY.md)
- Historial de cambios: [CHANGELOG](CHANGELOG.md)

- Avisos de terceros: [THIRD_PARTY_NOTICES](THIRD_PARTY_NOTICES.md)

## Registros históricos

Proceso de depuración en la TB321FU (2026-10-27, imagen base ap3a):

| Versión | Resultado | Causa y corrección |
| --- | --- | --- |
| v1 | system_server falla en bucle | d8 usó `--no-desugaring`, `IncompatibleClassChangeError`; se quitó ese parámetro |
| v2 | Falla la página de registro | `USE_BIOMETRIC_INTERNAL` es de nivel signature; se modificaron 57 comprobaciones |
| v3 | La página de registro sigue fallando | `MANAGE_BIOMETRIC` también es de nivel signature; se cambió a autorizar según la identidad |
| v4 | Desbloquea las primeras veces y luego se rechaza la cámara | CAMERA foreground-only; se cambiaron los parámetros de enlace |
| v5 | Todo correcto | |

Evolución posterior:

- 2026-10-27: la parte general se separó en [paranoidsense-gsi-port](https://github.com/CharlesGool/paranoidsense-gsi-port).
- 2026-11-01: en TB321FU-GSI-Mods, 3 parches se fusionaron en las 3 partes de `faceunlock`.
- 2026-11-05: se cambió el nombre del paquete a `com.android.faceunlock`; se separaron por ID de imagen base las versiones ap3a, bp1a (GSI oficial de Android 15) y cp3a (Android 17); este repositorio se separó de Mods.
