<div align="center">

<img src="logo_mbn.png" alt="Ministerio de Bienes Nacionales" width="150">

# Insumos del MCT

**Minuta y Plano Catastral Territorial**

División del Catastro Nacional · Departamento de Estudios Territoriales
Ministerio de Bienes Nacionales · Gobierno de Chile

<br>

![QGIS](https://img.shields.io/badge/QGIS-3.44%20%7C%204.2-93b023?style=flat-square&logo=qgis&logoColor=white)
![Verificación](https://img.shields.io/badge/verificaci%C3%B3n-sha256-0F69B5?style=flat-square)
![Cobertura](https://img.shields.io/badge/cobertura-16%20regiones-EC3C47?style=flat-square)
![Licencia](https://img.shields.io/badge/uso-institucional-6E7A87?style=flat-square)

</div>

---

Este repositorio contiene los **insumos cartográficos y normativos** que consume
el **MCT-SCRIPT**, la herramienta con que las regiones elaboran el Análisis
Catastral Territorial y la Minuta Catastral Territorial dentro de QGIS.

No es código: es aquello que el código necesita para producir un entregable
uniforme en todo el país. Publicarlo aquí permite que **Nivel Central actualice
una vez y las dieciséis regiones lo reciban**, con verificación de integridad y
sin que nadie copie archivos a mano.

## Contenido

| Archivo | Qué es |
|---|---|
| **`manifiesto_mbn.json`** | Índice maestro: versión, URL y `sha256` de cada insumo |
| **`plantilla_mbn_4_2.qpt`** | Plantilla de composición del plano. Compatible con QGIS 3.44 y 4.2 |
| **`L2_PROPUESTA.json`** | Reglas narrativas del análisis territorial |
| **`logo_mbn.png`** | Logotipo institucional |

## Cómo lo consume el script

El MCT-SCRIPT **no descarga en cada corrida**. El módulo `mbn_activos.py`
mantiene una copia local en el perfil de QGIS y solo consulta la red cuando hace
falta.

```
1ª corrida       →  descarga los insumos y los guarda
2ª en adelante   →  usa la copia local, sin tocar la red
cada 12 horas    →  baja solo el manifiesto (~1 KB) y compara los sha256;
                    descarga únicamente lo que cambió
sin internet     →  usa la copia local y avisa
```

Con eso el análisis **funciona en terreno, sin conexión**, y deja de esperar
cuatro descargas cada vez que se ejecuta.

## Verificación de integridad

El control de versiones es por **`sha256`**, no por número. El campo `version`
del manifiesto es informativo; quien manda es el hash.

Antes de guardar cualquier descarga, el script comprueba que el contenido calce
con el hash declarado. **Si no calza, no lo escribe.** Un archivo corrupto a
medio bajar, o alterado en tránsito, nunca llega a producir un plano.

> Por eso `.gitattributes` marca los insumos como `-text`: si Git normalizara
> los finales de línea cambiaría los bytes, el hash dejaría de calzar y el
> script rechazaría sus propias descargas.

## Publicar una versión nueva

Desde `QGIS_4.2/BANCO_PRUEBAS_4_2/`:

```bash
py _corregir_plantilla.py    # regenera plantilla y logotipo si corresponde
py _publicar_activos.py      # recalcula los sha256 y arma el paquete
```

Se copia el resultado a este repositorio y:

```bash
git add -A
git commit -m "insumos: <qué cambió>"
git push
```

Cada equipo lo detecta en su próxima revisión, sin intervención.

## La plantilla y QGIS 4.2

`plantilla_mbn_4_2.qpt` corre en **3.44 y en 4.2**. Respecto de la versión
anterior:

| Cambio | Por qué |
|---|---|
| Sin ítems HTML | Qt6 eliminó QtWebKit; en QGIS 4.2 se dibujaban como un recuadro rojo de error sobre el plano |
| Pesos de fuente en escala Qt6 | `fontWeight="75"` era *Bold* en Qt5, pero en Qt6 queda **por debajo de Thin**: los títulos perdían la negrita |
| Logotipo como ítem de Imagen | con su proporción real; antes se dibujaba estirado un 9 % |
| Marco exterior como Forma | lo dibujaba el ítem HTML vacío que hubo que retirar |

## Contexto

Forma parte de la transición del MCT a **QGIS 4.2 LTR**, que reemplaza a la
serie 3.44 a partir de octubre de 2026.

---

<div align="center">
<sub>Ministerio de Bienes Nacionales · División del Catastro Nacional · Chile</sub>
</div>
