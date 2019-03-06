# Unidad 3: Manejo de datos rasters Radar SAR Sentinel 1

Contenidos: Seleccionar colecciones, filtros por áreas,y por fechas. Construir máscaras. Visualización. Cómo exportar imágenes (ventajas y limitaciones del servicio). Funciones de agregación. Generación de expresiones. Extracción de información a partir de features (agregación por medias, máximos, mínimos, etc.). Exportar como imágenes.

 - Script de ejemplo __Radar SAR Sentinel 1__: [Ver](https://code.earthengine.google.com/c79b38cf504a0030170775d2bb547aa4)
 
## Trabajar con colecciones de imágenes

En la plataforma existe una gran cantidad de fuentes de información entre las que se incluye tanto información de base como imágenes satelitales, De sensores activos como son los sensores RADAR SAR.

En este caso vamos a seleccionar un producto y lo vamos a filtrar (acotar) a las necesidades particulares (intervalo de tiempo y área de interés), ya que generalmente el alcance es global y se disponen largas series temporales.

Cada producto tiene un código asociado (ImageCollection ID) y una nomenclatura de polaridad. El buscador del Code Editor permite ver las colecciones disponibles, el ID y la polaridad, presenta una breve explicación del producto y el origen del mismo:


### Escoger una colección

Seleccionaremos una colección, en este caso: **Sentinel-1 SAR GRD**. El objeto que se utiliza para representar colecciones de imágenes es un [ee.ImageCollection](https://developers.google.com/earth-engine/api_docs#eeimagecollection).

```javascript
// Seleccionar producto. Indicar el ImageCollection ID
var producto = ee.ImageCollection(''COPERNICUS/S1_GRD'');
```

Cada elemento de la colección instanciada en la variable _producto_ es a su vez un objeto de tipo [ee.Image](https://developers.google.com/earth-engine/api_docs#eeimage).


### Filtrar las escenas de interés

A partir de la colección de imágenes instanciada vamos a aplicar diferentes filtros y selecciones.
Recordemos:
 - Los filtros aplican sobre metadatos, valores de la imagen o utilizando geometrías.
 - Las selecciones aplican sobre las bandas.

Comenzamos definiendo un área de estudio a partir de un vector para luego poder filtrar por región.

 ```javascript
 // área de estudio (de sección anterior)
 var geometry = ee.FeatureCollection('ft:1NOdzgdcCiWZ6YcoEharXG_IYmW03G-ZJeUSZtoOB');
 ```

Ahora vamos a aplicar varios filtros, sigamos el script:

```javascript

// Filtrar colección
var producto_filtrado = producto
    // Por área de estudio. Debe estar cargada el área
    // en este caso en la variable “geometry”
    .filterBounds( geometry )

    //por rango de fechas
    .filterDate('2016-08-01', '2016-10-31')

    // por el modo del instrumento
    .filterMetadata('instrumentMode','equals','IW')

   //Por el paso de la orbita
   .filterMetadata('orbitProperties_pass','equals','ASCENDING')

  // Selecionando la Polaridad
   .select(['VV']); 

```

En la consola se pueden ver los detalles de la colección filtrada o seleccionada. Para ello hay que invocar a la función **print()** desde el Code Editor:

```javascript
// ver detalles de colección y filtros aplicados
print("Coleccion seleccionada", producto_filtrado);
```


Veamos cómo se publiac en un layer en el mapa.
Podemos visualizar la imagen identificando las polaridades a mostrar (orden V,H), seleccionar los valores máximos y mínimos para ecualizar cada banda y una descripción de la capa. Mover el visor hacia el área de estudio para ver la imagen.


```javascript
// Definir bandas a seleccionar
Map.addLayer(radar1, {bands: ['VV'], min: -6, max:-0.5,palette:['FFFFFF','CE7E45','DF923D','F1B555','FCD163','998718','66A000','529400']}, 'VV')
 ;
```


// centrar en area de estudio - indicar nivel de zoom
Map.setCenter(-79.39768715569062, -1.7434849510029904, 13);
```

La información obtenida puede ser exportada como image:


```
javascript
Export.image.toDrive({
  image:corte,
  description:'teos_clementina',
  region:geometry,
  crs:'EPSG:32717',
  maxPixels:1e12,
  scale:10
 });

```


