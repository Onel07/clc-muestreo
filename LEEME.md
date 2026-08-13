# CLC Muestreo — app de campo para teléfono

Captura de muestras de cobertura de la tierra según **Corine Land Cover, niveles 1 y 2**,
sobre imagen satelital, con GPS del teléfono y exportación a formatos ArcGIS.
Funciona **sin conexión** una vez instalada y con el área descargada.

## Archivos

| Archivo | Para qué sirve |
|---|---|
| `index.html` | La aplicación completa (mapa, GPS, captura, exportación). No usa librerías externas. |
| `sw.js` | Hace que la app abra sin internet después de la primera visita. |
| `manifest.webmanifest` | Permite instalarla como app en la pantalla de inicio. |
| `icon-192.png`, `icon-512.png` | Iconos de la app. |

Los cuatro archivos deben quedar **en la misma carpeta**.

## Instalación en el teléfono (una sola vez, con internet)

La carpeta necesita estar publicada en una dirección **https** (o `localhost`). No basta con
copiar el archivo al teléfono: abierto como archivo suelto (`file://`), el navegador **bloquea
el GPS y el almacenamiento**, que es justo lo que la app necesita.

1. Sube la carpeta a un alojamiento estático gratuito con https: GitHub Pages, Netlify Drop,
   Cloudflare Pages o el servidor de tu institución.
2. Abre la dirección en el teléfono con **Chrome** (Android) o **Safari** (iPhone).
3. Instálala:
   - **Android**: menú ⋮ → *Instalar aplicación* (o el botón 📲 *Instalar app* dentro de la app, en ☰ Mapa).
   - **iPhone**: botón Compartir ⬆ → *Añadir a pantalla de inicio*.
4. Ábrela desde el icono de la pantalla de inicio y concede el permiso de **ubicación**.
5. En ☰ Mapa pulsa **🔒 Proteger datos** para que el sistema no borre tus muestras ni las
   imágenes descargadas cuando quede poco espacio.

## Antes de salir a campo

1. Con internet, navega hasta tu zona de trabajo.
2. ☰ Mapa → **Descargar área**: elige el zoom máximo (17 ≈ 1,2 m/píxel es suficiente para
   fotointerpretación; 18–19 para detalle fino) y pulsa *Calcular* para ver cuántas teselas y
   cuánto espacio ocupará. Luego **⬇ Descargar área**.
3. Repite por cada zona. Puedes comprobar lo guardado con *Ver caché*.

Desde ese momento la app abre y dibuja el mapa **sin ninguna conexión**.

## En campo

- Arrastra para desplazarte; pellizca con dos dedos para el zoom.
- **◎** sigue tu posición · **☀** sube el contraste a pleno sol.
- **⊕ Capturar**: elige la posición (GPS o centro de la mira), Nivel 1 → Nivel 2, confianza,
  foto opcional y observaciones.
- **🎚 Promediar GPS**: toma 10 lecturas y usa la media. Útil bajo dosel o entre edificios,
  donde una sola lectura salta varios metros. La muestra queda marcada como `GPS_PROM` y el
  campo `PREC_M` guarda la dispersión real medida.
- Toca un punto del mapa para ver, editar o eliminar la muestra.
- La pantalla se mantiene encendida mientras trabajas (se puede desactivar en ☰ Mapa).

## Exportar a ArcGIS

Todo sale en coordenadas geográficas **WGS 84 (EPSG:4326)**.

| Formato | Cómo se usa en ArcGIS |
|---|---|
| **Shapefile** (`.zip` con shp+shx+dbf+prj+cpg) | Descomprimir y arrastrar el `.shp` al mapa. |
| **Esri JSON** (FeatureSet) | Herramienta `JSON To Features` (Conversion Tools). |
| **GeoJSON** | `JSON To Features`, o se abre directamente. |
| **CSV** | `XY Table To Point` con X=`LON`, Y=`LAT`, GCS_WGS_1984. |

Campos de la tabla: `ID, FECHA, HORA, N1_COD, N1_NOM, N2_COD, N2_NOM, LON, LAT, ALT_M,
PREC_M, FUENTE, CONF, OBS, FOTO`.

Si adjuntaste fotos, el ZIP del shapefile incluye la carpeta `fotos/` y el campo `FOTO`
guarda la ruta relativa, que sirve como hipervínculo en ArcGIS.

En el teléfono, la casilla *Compartir con otra app* envía el archivo por correo, Drive o
WhatsApp en vez de dejarlo en Descargas.

## Leyenda incluida

Nivel 1 (5 clases) y Nivel 2 (15 clases) de la nomenclatura Corine Land Cover, con los
colores oficiales de la leyenda:

1. Territorios artificializados — 1.1 Zonas urbanizadas · 1.2 Zonas industriales o comerciales
   y redes de comunicación · 1.3 Zonas de extracción minera y escombreras · 1.4 Zonas verdes
   artificializadas, no agrícolas
2. Territorios agrícolas — 2.1 Cultivos transitorios · 2.2 Cultivos permanentes · 2.3 Pastos ·
   2.4 Áreas agrícolas heterogéneas
3. Bosques y áreas seminaturales — 3.1 Bosques · 3.2 Áreas con vegetación herbácea y/o
   arbustiva · 3.3 Áreas abiertas, sin o con poca vegetación
4. Áreas húmedas — 4.1 Áreas húmedas continentales · 4.2 Áreas húmedas costeras
5. Superficies de agua — 5.1 Aguas continentales · 5.2 Aguas marítimas

Si trabajas con la adaptación colombiana para Colombia escala 1:100.000, los códigos de
nivel 1 y 2 coinciden; solo cambian algunos nombres de nivel 3 en adelante, que esta app no usa.

## Imágenes de fondo

Esri World Imagery (por defecto), Esri Imagery Clarity, OpenStreetMap o sin fondo.
Atribución obligatoria al publicar mapas derivados: *Esri, Maxar, Earthstar Geographics*
para las imágenes de Esri, *© OpenStreetMap contributors* para OSM.

## Copia de seguridad

Las muestras viven en el teléfono. Exporta con frecuencia; el GeoJSON exportado se puede
volver a importar (📥 *Importar GeoJSON* en la lista de muestras) para retomar el trabajo o
pasarlo a otro dispositivo.
