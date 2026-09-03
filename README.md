# Activos del MCT — Ministerio de Bienes Nacionales

Plantilla de composición, reglas narrativas y logotipo institucional que consume
el **MCT-SCRIPT** (Análisis Catastral Territorial y Minuta Catastral Territorial).

División del Catastro Nacional · Departamento de Estudios Territoriales

## Qué hay aquí

| Archivo | Qué es |
|---|---|
| `manifiesto_mbn.json` | índice: versión, URL y `sha256` de cada activo |
| `plantilla_mbn_4_2.qpt` | plantilla de composición, compatible con QGIS 3.44 y 4.2 |
| `L2_PROPUESTA.json` | reglas narrativas del análisis territorial |
| `logo_mbn.png` | logotipo institucional |

## Cómo lo usa el script

El script **no descarga en cada corrida**. `mbn_activos.py` mantiene un caché en
el perfil de QGIS (`<perfil>/MBN_ACTIVOS/`) y:

1. La primera vez descarga todo.
2. Después usa el caché, sin tocar la red.
3. Cada 12 horas baja solo `manifiesto_mbn.json` (~1 KB) y compara los `sha256`.
   Descarga únicamente lo que cambió.
4. Sin internet, usa el caché y avisa.

**El control de versiones es por `sha256`, no por número.** El campo `version` es
informativo; quien manda es el hash. Además el script verifica el hash de lo
descargado *antes* de guardarlo: si no calza, no lo escribe.

## Publicar una versión nueva

Desde `QGIS_4.2/BANCO_PRUEBAS_4_2/`:

```bash
py _corregir_plantilla.py    # regenera plantilla y logo si hace falta
py _publicar_activos.py      # recalcula sha256 y arma PARA_SUBIR/
```

Copiar el contenido de `PARA_SUBIR/` aquí, y:

```bash
git add -A && git commit -m "activos: <qué cambió>" && git push
```

## Compatibilidad de la plantilla

`plantilla_mbn_4_2.qpt` corre en **3.44 y en 4.2**. Respecto de la original:

- sin ítems HTML — en Qt6 no existe QtWebKit y salían como recuadro rojo
- pesos de fuente traducidos a la escala de Qt6 (`75` → `700` + `namedStyle`)
- logotipo como ítem de Imagen, con su proporción real
- marco exterior como Forma, que antes dibujaba el ítem HTML vacío
