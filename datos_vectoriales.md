# Unidad 1: Manejo de datos vectoriales

Contenidos: Manejo de datos vectoriales: Manejo de geometrías y generación de features. Creación y administración de colecciones de features. Carga y visualización de vectores utilizando Google Fusion Table (FT). Manejo de  iteraciones sobre colecciones de features. Exportar como tabla de datos. Realizar gráficos.

## Manejo de geometrías y generación de features

Earth Engine maneja datos vectoriales con el tipo [ee.Geometry](https://developers.google.com/earth-engine/api_docs#eegeometry). La especificación [GeoJSON](https://tools.ietf.org/html/rfc7946) describe en detalle el tipo de geometrías soportadas por Earth Engine, incluyendo:

-   **Point**: una lista de coordenadas en alguna proyección,
-   **LineString**: una lista de puntos,
-   **LinearRing**: una LineString cerrada
-   **Polygon**: una lista de LinearRings donde la primera es una cáscara y los anillos subsiguientes son agujeros.

Earth Engine también soporta **MultiPoint**, **MultiLineString** y **MultiPolygon**. GeoJSON GeometryCollection también es compatible, aunque tiene el nombre MultiGeometry dentro de Earth Engine.

### Creación de Geometrías

Vamos a crear y mapear una geometría

```javascript
    var point = ee.Geometry.Point([-60.54, -31.85]);
```

[ee.Geometry.Point](https://developers.google.com/earth-engine/api_docs#eegeometrypoint) es la llamada a la API Earth Engine que recibe dos parámetros una lista
**ee.List([])** y el segundo que es opcional una proyección que puede ser especificada como código EPSG [[1]](#ftnt1). El valor predeterminado es EPSG:4326 (WGS84 Lat/Lon)

La definición de esa geometría (según la definición [GeoJSON](https://tools.ietf.org/html/rfc7946)) sería:

```javascript
    {
      "geodesic": true, // WGS84 Lat/Lon
      "type": "Point",
      "coordinates": [
        -79.36,
        -1.71
      ]
    }
```

De esa forma los datos son almacenados y utilizados internamente por la librería. Para poder ver en el Code Editor el formato nativo podemos hacer **print(point);** y luego en la consola hacer clic en JSON.

También podemos agregar la geometría en el mapa utilizando la siguiente
instrucción:

```javascript
    Map.addLayer(point,{'color':'00FF11'} ,'Punto');
```

**Tips**: Si quiere modificar el color de la geometría el formato hexadecimal de colores html puede obtenerse desde Google realizando la búsqueda con [picker color](https://htmlcolorcodes.com/es/selector-de-color/).

El resto de las geometrías se construyen de la misma forma, veamos:

-   Líneas

```javascript
   var lineString = ee.Geometry.LineString([[-79.56,0.031], [-80.15,-1.37], [-78.81,-0.71], [-77.56,-0.71]]);
```

[ee.Geometry.LineString](https://developers.google.com/earth-engine/api_docs#eegeometrylinestring) recibe una lista de puntos y parámetros opcionales.

Agregue la geometría al mapa:

```javascript
    Map.addLayer(lineString,{'color':'CC0011'} ,'Linea');
```

Podemos centrar el mapa en una geometría o feature particular, eso lo podemos hacer con Map.centerObject así:

```javascript
    Map.centerObject(lineString, 7);
```

-   Anillo de línea

```javascript
   var linearRing = ee.Geometry.LinearRing(
[[-80.11,-1.29],[-80.29,-2.01],
[-79.91,-2.10],[-79.47,-2.07],
[-79.26,-1.65],[-79.67,-1.33],
[-80.11,-1.29]]);
```
[ee.Geometry.LinearRing](https://developers.google.com/earth-engine/api_docs#eegeometrylinearring) recibe una lista de puntos que a diferencia de LineString comienza y termina con el mismo punto para poder cerrar el anillo. También tiene parámetros opcionales.

Y si, al mapa!!

```javascript
    Map.addLayer(linearRing,{'color':'b227b0'} ,'Anillo');
```

-   Rectángulo

```javascript
    var rectangle = ee.Geometry.Rectangle([-79,-1,-80,0]);
```

[ee.Geometry.Rectangle](https://developers.google.com/earth-engine/api_docs#eegeometryrectangle) recibe una lista con esquinas mínimas y máximas del rectángulo, como una lista de dos puntos en formato de coordenadas GeoJSON 'Point' o una lista de dos ee.Geometry que describen un punto, o una lista de cuatro números en el orden __xMin, yMin , xMax, yMax__.

A mapear!!

```javascript
    Map.addLayer(rectangle,{'color':'fbff23'} ,'Rectángulo');
```

-   Polígono

```javascript
    var vertice =[[-80.19,-2.23],[-79.71,-2.34],[-79.71,-1.98],[-80.24,-1.92],[-80.19,-2.23]];
    var poligono = ee.Geometry.Polygon( vertices );
```

[ee.Geometry.Polygon](https://developers.google.com/earth-engine/api_docs#eegeometrypolygon) recibe una lista de anillos que definen los límites del polígono. Puede ser una lista de coordenadas en el formato 'Polygon' de GeoJSON, o una lista de ee.Geometry que describe un LinearRing. El resto de los parámetros son similares al resto de las geometrías.

```javascript
    Map.addLayer(poligono, {'color':'16a322'} ,'Polígono');
```

-   Geometrías Multiparte: Una geometría individual puede consistir en múltiples geometrías. Para dividir una Geometría de varias partes en sus geometrías constitutivas, use **geometry.geometries()**. Ejemplo:

```javascript
    var multiPoint = ee.Geometry.MultiPoint(
          [
            [-62.319,-32.856],
            [-62.528,-32.944],
            [-62.418,-33.109],
            [-62.193,-33.008],
            [-62.166,-32.805]
            ]);

    Map.addLayer(multiPoint, {'color':'16a322'} ,'multiPoint');

```

Vamos a imprimir los puntos de  la geometría, así que al objeto multiPoint le vamos a pedir todas las geometrías que lo componen.

```javascript
    var las_geometrias = multiPoint.geometries();
    print(las_geometrias);
```

1.  ¿Qué tipo de dato es la variable **las\_geometrias**?
2.  ¿Cómo puedo recuperar una de las geometrías contenidas en
    **las\_geometrias**? lista.get(id)

**Desafío 1:** Convierta los puntos existentes en multiPoint a un LinearRing.

## Crear Geometrías desde el mapa

Existe una forma muy práctica de dibujar Geometrías desde el mismo mapa del Code Editor.

Veamos un ejemplo:

|  |  |
| - | - |
| Las opciones para dibujar están ubicadas en el sector superior izquierdo del mapa. <br/>Las herramientas disponibles permiten activar el dibujado de geometrías múltiples de: puntos, líneas y polígonos. <br/>Para dejar de dibujar se hace clic en la mano de la izquierda. | ![Selección\_508.png](images/image18.png) |
| Una vez que se activa la herramienta esta se habilita para poder dibujar. <br/>Se asigna un color al azar y cada figura que se trace formará parte de una geometría múltiple. | ![Selección\_509.png](images/image28.png) |
| Es posible incorporar desde la sección de Geometry Imports una nueva capa.<br/>Esta se instancia como una nueva variable de la clase Geometry. | ![Selección\_510.png](images/image1.png) |
| Las capas de geometrías que se van incorporando serán ubicadas en la sección de Imports del editor de código fuente. <br/>Estos objetos son mostrados de manera formateada. Pero haciendo clic en el ícono azul se muestra el código fuente correspondiente para la creación de la geometría. | ![](images/image7.png) |
| El código fuente generado puede copiarse y pegarse en el script que se está escribiendo. <br/>**Ojo**: En algunos Navegadores no copia (Firefox 49.0.2, por ejemplo.) | ![Selección\_512.png](images/image24.png) |


## Geometrías Geodésicas y planas

Una geometría creada en Earth Engine es geodésica (es decir, los bordes son la trayectoria más corta en la superficie de una esfera) o planar (es decir, los bordes son el camino más corto en un plano cartesiano bidimensional).

En la configuración predeterminada de la instanciación de un objeto ee.Geometry.XXXXX este se crea como EPSG:4326, es decir, será una geometría geodésica.

```javascript
var PoligonoGeo = ee.Geometry.Polygon([
[
    [-71.411,-39.470], [-57.128,-39.402],
    [-57.304,-33.394], [-70.751,-33.358],
    [-71.411,-39.470]]
]);
```

Esa misma geometría puede ser expresada en el plano, ya sea cuando se crea o puede ser convertida:

```javascript
var PoligonoPlano = ee.Geometry(PoligonoGeo, null, false);
```

Veámos cómo se ven estas geometrías en el mapa:

```javascript
Map.centerObject(PoligonoGeo);
Map.addLayer(PoligonoGeo, {color: 'FF0000'}, 'Geodésico');
Map.addLayer(PoligonoPlano, {color: '000000'}, 'Plano');
```

## Operaciones con Geometrías


Earth Engine admite una amplia variedad de operaciones en objetos Geometry. Estos incluyen operaciones en geometrías individuales tales como calcular un buffer, centroide, bounding box, perímetro, envolvente convexa, etc.

Utilizando la definición del polígono de la anterior vamos a realizar
algunas operaciones de geometrías:

-   Cálculo de área en KM²

```javascript
print('Área: ', poligono.area().divide(1000 * 1000));
// Todos los valores de mediciones de distancias vienen expresados en metros.
```
-   Longitud de perímetro en KM

```javascript
print('Perímetro: ', poligono.perimeter().divide(1000));
```
-   Mostrar el GeoJSON 'type'.

```javascript
print('Geometry type: ', poligono.type());
```

-   Mostrar la lista de coordenadas.

```javascript
print('Coordenadas: ', poligono.coordinates());
```

-   Muestra true/false si las coordenadas son o no geodésicas.

```javascript
print('¿Está en coordenadas geodésicas? ', poligono.geodesic());
```

-   Muestra el BBOX de una geometría

```javascript
print('Bounding Box', poligono.bounds());
// bounds retorna el rectángulo que envuelve a la geometría en una geometría plana.
```

-   Calcular el buffer de un polígono

```javascript
var buffer = poligono.buffer(5000);
// La distancia del buffer está expresada en metros.
```

-   Calcular el centroide de un polígono..

```javascript
var centroid = poligono.centroid();
```
 
Ahora podemos mapear algunas de estas operaciones:

```javascript
Map.addLayer(buffer, {'color':'0be51e'}, 'buffer');
Map.addLayer(poligono, {}, 'el polígono');
Map.addLayer(centroid, {'color':'e5280b'}, 'centroide');
Map.centerObject(buffer, 7);
```

Las  operaciones que hemos realizado han sido todas con operadores unitarios, donde a la geometría le pedimos (o le calculamos) algo. Ahora vamos a probar algunos operadores entre geometrías.

Comenzamos con dos geometrías de polígono que las he creado desde el mapa como se mostró previamente.

```javascript

var poli1 = ee.Geometry.Polygon(
      [[[-79.6244546527131,-1.4476098415359253],
       [-79.73294464294747,-1.4489826938400079],
       [-79.83456817810372,-1.6329370607280587],
       [-79.62582794372872,-1.6343097935796802],
       [-79.6244546527131,-1.4476098415359253]]]);

var poli2 = ee.Geometry.Polygon(
      [[[-79.66702667419747,-1.4023052538671108],
          [-79.67801300232247,-1.6315643269388478],
          [-79.46789947693185,-1.6398007155944714],
          [-79.46377960388497,-1.3981866110774313],
          [-79.66702667419747,-1.4023052538671108]]]);
```
Calculamos la unión de las dos geometrías. Donde el primer parámetro es la geometría que se quiere unir y el segundo es un margen de error. **ErrorMargin** es la cantidad máxima de error tolerada al realizar cualquier reproyección necesaria, el valor está expresado en metros.


```javascript
var poli1Upoli2 = poli1.union(poli2, ee.ErrorMargin(1));

Map.addLayer(poli1Upoli2, {color: 'FF00FF'}, 'Unión');        

// Centramos el mapa en la unión
Map.centerObject(poli1Upoli2, 12);
```
 

Calculamos la intersección de los dos polígonos con la función intersection que

```javascript
var intersection = poli1.intersection(poli2, ee.ErrorMargin(1));
Map.addLayer(intersection, {color: '00FF00'}, 'Intersección');
```
 

Calculamos la diferencia entre el polígono 1 y el polígono 2. Es el área de la primer geometría que no comparte con la segunda.

```javascript
var diff1 = poli1.difference(poli2, ee.ErrorMargin(1));
Map.addLayer(diff1, {color: 'FFFF00'}, 'Diferencia( P1 - P2)');
```

Calculamos la diferencia simétrica, esta se define como el área de la geometría A y el área de la geometría B excepción el área común a ambas.

```javascript
var dif_sim = poli1.symmetricDifference( poli2, ee.ErrorMargin(1));
Map.addLayer(dif_sim, {color: '000000'}, 'Diferencia Simétrica');

// Mapeamos las dos geometrías con las que hicimos los ejemplos.
Map.addLayer(poli1, {color: 'FF0000'}, 'Polígono 1');
Map.addLayer(poli2, {color: '0000FF'}, 'Polígono 2');
```

**Desafío 2:** Verifique si una de las rectas que pasan por los puntos **[-63.635, -25.051]** y **[-63.617, -25.146]** se intersectan en la intersección de las geometrías utilizadas previamente (poli1 y poli2).

**Desafío 3:** Ahora compruebe si el punto definido abajo está contenido en la geometría que resultó de la diferencia simétrica.

```javascript
var punto = ee.Geometry.Point([-63.6084366, -25.0803128]);
```

## Creación de Features

Un Feature de Earth Engine se define como un GeoJSON Feature. En pocas palabras, es un objeto con una propiedad de tipo Geometry y una propiedad de properties que almacena un diccionario de otras propiedades.

Entonces, necesitamos un objeto Geometry y opcionalmente un diccionario con los atributos de ese Feature.

```javascript

// La geometría
var poligono = ee.Geometry.Polygon(
        [[[-79.66702667419747,-1.4023052538671108],
          [-79.67801300232247,-1.6315643269388478],
          [-79.46789947693185,-1.6398007155944714],
          [-79.46377960388497,-1.3981866110774313],
          [-79.66702667419747,-1.4023052538671108]]]);
```

La declaración del Feature:

```javascript
var miFeature = ee.Feature(poligono,
    {variable_1: 100,
     variable_2: 'Hola'});
```

Al igual que con las geometrías podemos enviarlos a la consola utilizando print o mostrarlos en el mapa.

```javascript    
print(miFeature);
Map.addLayer(miFeature, {}, 'Mi Feature!');
Map.centerObject(miFeature, 12);    
```

La geometría del Feature puede ser nula y se podría crear el Feature solo con un diccionario:

```javascript
var dict = {distancia: ee.Number(10).add(150), lugar: 'Puebloviejo'};
var featureSinGeo = ee.Feature(null, dict);
```

Los Features tienen las mismas funcionalidades para gestionar sus geometrías que los objetos Geometry. Además, poseen otros métodos setters & getters para el manejo de las propiedades.

```javascript
var feature_ejemplo = ee.Feature(
      ee.Geometry.Point([-79.3283, -1.7819]))
        .set('Nombre', 'Montalvo')
        .set('Altura', 100);

// Recupero una propiedad del feature
var nombre = feature_ejemplo.get('Nombre');
print(nombre);

// Asigno una nueva propiedad
feature_ejemplo = feature_ejemplo.set('Población', 75000);

// Sobreescribo un nuevo diccionario
var newDict = {Nombre: 'El Lote', Altura: 300};
var feature_ejemplo = feature_ejemplo.set(newDict);

// Se muestran los resultados
print(feature_ejemplo);

Map.addLayer(feature_ejemplo, {}, 'Ejemplo 2');

```

### 4. FeatureCollection de una muestra al azar

Es posible crear un FeatureCollection a partir de generar una muestra al azar [ee.FeatureCollection.randomPoints](https://developers.google.com/earth-engine/api_docs#eefeaturecollectionrandompoints) de puntos dada una región.

```javascript

var region = ee.Geometry.Rectangle(-80.051, -1.236, -79.618,-1.531);
var randomPoints = ee.FeatureCollection.randomPoints( region,
    100, // cantidad de puntos
    123); // seed

print(randomPoints)

Map.centerObject(randomPoints);

Map.addLayer(randomPoints, {}, 'Puntos al azar');
```

### 5.  Creación de FeaturesCollection desde el Mapa

Al igual que mostramos anteriormente como dibujar geometrías, es posible definir FeaturesCollection dibujando desde el mapa.

![](images/image17.png)        

En la configuración podemos darle un nombre al Layer y agregar propiedades con valores por defecto y definir el color.

![](images/image29.png)

![](images/image27.png)

Podemos incorporar esto FeaturesCollection desde la sección de Imports.

![](images/image14.png)

## Operador Selection

Selección de propiedades de un Feature para generar un nuevo FeatureCollection.

**Importante:** las selecciones aplican sobre las __columnas__, es decir, sobre los atributos de una colección de features o las bandas cuando veamos colecciones de imágenes.


```javascript

var fc_seleccion = filtrados.select( ['area_ha', 'class'], ['AREA','CLASE']);
print(fc_seleccion);
```

## Exportar como tabla de datos

Para exportar un FeatureCollection a Google Drive se requiere la instrucción Export.table.toDrive()

![](images/image26.png)

### KML

```javascript
Export.table.toDrive({
          collection: <Nombre del FC>,
          description:'Una descripción de la capa para encontrarlo en Drive',
          fileFormat: 'KML o KMZ'
});
```

### CSV

```javascript
Export.table.toDrive({
    collection: <Nombre del FC>,
    description:'Una descripción de la tabla para encontrarla en Drive',
    fileFormat: 'CSV'
});
```

Ejemplo:

```javascript
var key = 'ft:1t-2SIDNQHZji_6iSWww0pAbd_4i33l8o68NUh4an';
var muestreos = ee.FeatureCollection(key);

var mapear_clase = function( elemento ){
return elemento.set('tipo',
ee.Algorithms.If( ee.Number(elemento.get('class')).gte(20).and(ee.Number(elemento.get('class')).lte(23))  ,
      'Bosque',
      'No Bosque'));
};

var fc_tipo = muestreos.map( mapear_clase );

// Exportar a CSV
Export.table.toDrive({
collection: fc_tipo,
description: 'TablaBosqueNoBosque', //Es el nombre que tendrá el archivo
fileFormat: 'CSV'
});
```

Luego de exportar se incluirá una nueva tarea que hay que poner a
correr.

![](images/image11.png)

Dependiendo de la cantidad de features en la colección el proceso puede
demorar mucho o poco y además también dependerá de la carga de trabajo
de la plataforma.

![](images/image19.png)




Bibliografía
============

API | Google Earth Engine API. [https://developers.google.com/earth-engine/api\_docs](https://developers.google.com/earth-engine/api_docs)

* * * * *

[[1]](#ftnt_ref1) European Petroleum Survey Group
