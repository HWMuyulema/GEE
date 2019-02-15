<!-- $theme: default -->
<!-- footer: Taller GEE - para el monitoreo del uso y cobertura del suelo. Mendoza. Diciembre 2017 -->
<!-- page_number: true -->


Manejo de datos vectoriales
==




---

# Contenidos 

Manejo de datos vectoriales: Manejo de geometrías y generación de features. Creación y administración de colecciones de features. Carga y visualización de vectores utilizando Google Fusion Table (FT). Manejo de  iteraciones sobre colecciones de features. Exportar como tabla de datos. Realizar gráficos.

---

# Manejo de geometrías y generación de features

Earth Engine maneja datos vectoriales con el tipo [ee.Geometry](https://developers.google.com/earth-engine/api_docs#eegeometry). La especificación [GeoJSON](https://tools.ietf.org/html/rfc7946) describe en detalle el tipo de geometrías soportadas por Earth Engine, incluyendo:

-   **Point**: una lista de coordenadas en alguna proyección,
-   **LineString**: una lista de puntos,
-   **LinearRing**: una LineString cerrada
-   **Polygon**: una lista de LinearRings donde la primera es una cáscara y los anillos subsiguientes son agujeros.

Earth Engine también soporta **MultiPoint**, **MultiLineString** y **MultiPolygon**. GeoJSON GeometryCollection también es compatible, aunque tiene el nombre MultiGeometry dentro de **EE**.

---

# Creación de Geometrías

Vamos a crear y mapear una geometría

```javascript
var point = ee.Geometry.Point([-79.31, -1.76]);
```

[ee.Geometry.Point](https://developers.google.com/earth-engine/api_docs#eegeometrypoint) es la llamada a la API Earth Engine que recibe dos parámetros una lista **ee.List([])** y el segundo que es opcional una proyección que puede ser especificada como código EPSG [[1]](#ftnt1). 

El valor predeterminado es EPSG:4326 (WGS84 Lat/Lon)

---

# Representación interna
<br/>

La definición de esa geometría (según la definición [GeoJSON](https://tools.ietf.org/html/rfc7946)) sería:

```javascript
    {
      "geodesic": true, // WGS84 Lat/Lon
      "type": "Point",
      "coordinates": [
        -79.3169, 
	-1.76
      ]
    }
```

---
# Geometrías Disponibles

- Geometrías Simples
	- [ee.Geometry.LinearString](https://developers.google.com/earth-engine/api_docs#eegeometrylinearring) 
	- [ee.Geometry.LinearRing](https://developers.google.com/earth-engine/api_docs#eegeometrylinearring) 
 	- [ee.Geometry.Rectangle](https://developers.google.com/earth-engine/api_docs#eegeometryrectangle)
 	- [ee.Geometry.Polygon](https://developers.google.com/earth-engine/api_docs#eegeometrypolygon)
 	- [ee.Geometry.Rectangle](https://developers.google.com/earth-engine/api_docs#eegeometryrectangle)
 - Geometrías Múltiples
 	- [ee.Geometry.MultiPoint](https://developers.google.com/earth-engine/api_docs#eegeometrymultiPoint)
 	- [ee.Geometry.MultiLineString](https://developers.google.com/earth-engine/api_docs#eegeometrymultilinestring)
	- [ee.Geometry.MultiPolygon](https://developers.google.com/earth-engine/api_docs#eegeometrymultipolygon)

Veamos un ejemplo con todas [ [Ver](https://code.earthengine.google.com/ad526731fd0bdf18393df8fe303ca19f) ]

--- 
# Mapear e imprimir un objeto geometry

Un objeto Geometry puede ser inspeccionado  desde la consola mediante **print(objetoGeometry);**

También podemos __agregar la geometría en el mapa__ utilizando la siguiente instrucción:

```javascript
Map.addLayer(point,{'color':'00FF11'} ,'Punto');
```

**Tips**: Si quiere modificar el color de la geometría el formato hexadecimal de [colores html](http://htmlcolorcodes.com/es/) puede obtenerse desde Google realizando la búsqueda con [picker color](https://www.google.com.ar/#q=picker+color).

<!--

---
# Geometría de línea

El resto de las geometrías se construyen de la misma forma, veamos:

```javascript
var lineString = ee.Geometry.LineString([
	[-79.78, -1.53], 
        [-79.52, -1.73], 
        [-79.29, -1.71], 
        [-79.28, -1.79]]);
```

[ee.Geometry.LineString](https://developers.google.com/earth-engine/api_docs#eegeometrylinestring) recibe una lista de puntos y parámetros opcionales.

```javascript
//Agreganos la geometría al mapa
Map.addLayer(lineString,{'color':'CC0011'} ,'Linea');
```
-->

---
# TIPS: Centrar el mapa
Podemos centrar el mapa en una geometría o _feature_ particular, eso lo podemos hacer con Map.centerObject así:

```javascript
// 1er elemento el objeto georeferenciado
// 2do elemento el nivel de zoom 0 más lejos
Map.centerObject(lineString, 7);
```

<!--

---
# Geometría LinearRing

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
---
# Geometría Rectángulo   

```javascript
var rectangle = ee.Geometry.Rectangle([-79,-1,-80,0]);
```

[ee.Geometry.Rectangle](https://developers.google.com/earth-engine/api_docs#eegeometryrectangle) recibe una lista con esquinas mínimas y máximas del rectángulo, como una lista de dos puntos en formato de coordenadas GeoJSON 'Point' o una lista de dos ee.Geometry que describen un punto, o una lista de cuatro números en el orden __xMin, yMin , xMax, yMax__.

```javascript
Map.addLayer(rectangle,{'color':'fbff23'} ,'Rectángulo');
```
---
# Geometría Polígono

```javascript
var vertices = [ 
	[-80.19,-2.23],[-79.71,-2.34],[-79.71,-1.98],[-80.24,-1.92],[-80.19,-2.23]
	];

var poligono = ee.Geometry.Polygon( vertices );
```

[ee.Geometry.Polygon](https://developers.google.com/earth-engine/api_docs#eegeometrypolygon) recibe una lista de anillos que definen los límites del polígono. Puede ser una lista de coordenadas en el formato 'Polygon' de GeoJSON, o una lista de ee.Geometry que describe un LinearRing. El resto de los parámetros son similares al resto de las geometrías.

```javascript
Map.addLayer(poligono, {'color':'16a322'} ,'Polígono');
```

---
# Geometrías Multiparte

Una geometría individual puede consistir en múltiples geometrías. 

```javascript
var multiPoint = ee.Geometry.MultiPoint(
          [
           [-79.78, -1.53], 
 	   [-79.52, -1.73], 
 	   [-79.29, -1.71], 
	   [-79.28, -1.79]
            ]);

Map.addLayer(multiPoint,{'color':'16a322'},'multiPoint');
```
Para dividir una Geometría de varias partes en sus geometrías constitutivas, use **geometry.geometries()**.

-->

---
# Método _geometries_

Vamos a imprimir los puntos de  la geometría, así que al objeto multiPoint le vamos a pedir todas las geometrías que lo componen.

```javascript
    var las_geometrias = multiPoint.geometries();
    print(las_geometrias);
```

1.  ¿Qué tipo de dato es la variable **las\_geometrias**?
<!-- es lista -->
2.  ¿Cómo puedo recuperar una de las geometrías contenidas en
    **las\_geometrias**? 
<!-- lista.get(id) -->

---
# Desafío 1
Convierta los puntos existentes en multiPoint a un LinearRing.
![100% center](../images/puntos.png)

---
# Crear Geometrías desde el mapa

Existe una forma muy práctica de dibujar Geometrías desde el mismo mapa del Code Editor.

Veamos un ejemplo:

 - Las opciones para dibujar están ubicadas en el sector superior izquierdo del mapa. 
 - Las herramientas disponibles permiten activar el dibujado de geometrías de: puntos, líneas y polígonos. <br/>Para dejar de dibujar se hace clic en la mano de la izquierda. 

![120% center](../images/image18.png)

---
# Crear Geometrías desde el mapa

 - Una vez que se activa la herramienta esta se habilita un **nuevo Layer** para poder dibujar. 
 - Se asigna un color al azar y cada figura que se trace formará parte de una geometría múltiple. 

![100% center](../images/image28.png)

---
# Crear Geometrías desde el mapa

- Es posible incorporar desde la sección de __Geometry Imports__ una nueva capa. 
- Esta se instancia como una nueva variable de la clase Geometry.XXXX. 

![100% center](../images/image1.png)

---
# Sección _Imports_

- Las capas de geometrías que se van incorporando serán ubicadas en la sección __Imports__ del editor de código fuente. 
- Estos objetos son mostrados de manera formateada. Pero haciendo clic en el ícono azul se muestra el código fuente correspondiente para la creación de la geometría. 

![100% center](../images/image7.png)

Todo lo que esté en __Imports__ lo puedo compartir con el link.

---

# Código fuente del _Import_

El código fuente generado puede copiarse y pegarse en el script que se está escribiendo. 

![100% center](../images/image24.png)

**Ojo**: Solo funciona con Chrome! 



<!--

# Geometrías Geodésicas y Planas

- Una geometría creada en Earth Engine es **geodésica** (_los bordes son la trayectoria más corta en la superficie de una esfera_) o **planar** (_los bordes son el camino más corto en un plano cartesiano bidimensional_).

En la configuración predeterminada de la instanciación de un objeto ee.Geometry.XXXXX este se crea como EPSG:4326, es decir, será una geometría geodésica.



# Geometrías Geodésicas y planas

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
-->

---

# Operaciones básicas con Geometrías

- Earth Engine admite una amplia variedad de operaciones en objetos Geometry. 
- Estos incluyen operaciones unarias:
	- calcular un buffer, 
	- centroide, 
	- bounding box, 
	- perímetro, 
	- envolvente convexa, 
    - ...

---

# Operaciones con Geometrías

Utilizando la definición del polígono de la anterior vamos a realizar algunas operaciones de geometrías:

- Cálculo de área en KM²

```javascript
print('Área: ', poligono.area().divide(1000 * 1000));
// Todos los valores de mediciones de distancias 
// vienen expresados en metros.
```

---
# Operaciones con Geometrías

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
print('¿Está en coordenadas geodésicas? ', 
       poligono.geodesic());
```
---
# Operaciones con Geometrías

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

---

# Operaciones con Geometrías

Ahora podemos mapear algunas de estas operaciones:

```javascript
Map.addLayer(buffer, {'color':'0be51e'}, 'buffer');
Map.addLayer(poligono, {}, 'el polígono');
Map.addLayer(centroid, {'color':'e5280b'}, 'centroide');
Map.centerObject(buffer, 7);
```

---

# Operaciones con Geometrías: Binarias

Las  operaciones que hemos realizado han sido todas con operadores unarios, donde a la geometría le pedimos (o le calculamos) algo. Ahora vamos a probar algunos operadores entre geometrías.

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

# Operaciones con Geometrías

Calculamos la unión de las dos geometrías. Donde el primer parámetro es la geometría que se quiere unir y el segundo es un margen de error. **ErrorMargin** es la cantidad máxima de error tolerada al realizar cualquier reproyección necesaria, el valor está expresado en metros.


```javascript
var poli1Upoli2 = poli1.union(poli2, ee.ErrorMargin(1));

Map.addLayer(poli1Upoli2, {color: 'FF00FF'}, 'Unión');        

// Centramos el mapa en la unión
Map.centerObject(poli1Upoli2, 12);
```
---

# Intersección de geometrías
 

Calculamos la __intersección__ de los dos polígonos con la función intersection

```javascript
var intersection = poli1.intersection(
    poli2, 
    ee.ErrorMargin(1));
Map.addLayer(intersection, 
	{color: '00FF00'}, 'Intersección');
```

---
# Diferencia entre geometrías

Calculamos la diferencia entre el polígono 1 y el polígono 2. Es el área de la primer geometría que no comparte con la segunda.

```javascript
var diff1 = poli1.difference(poli2, ee.ErrorMargin(1));

Map.addLayer(
	diff1, 
    	{color: 'FFFF00'},
	'Diferencia( P1 - P2)');
```
---
# Diferencia Simétrica

Calculamos la diferencia simétrica, esta se define como el área de la geometría A y el área de la geometría B excepción el área común a ambas.

```javascript
var dif_sim = poli1.symmetricDifference( 
	poli2, 
    	ee.ErrorMargin(1));
Map.addLayer(dif_sim, 
		{color: '000000'}, 
        'Diferencia Simétrica');

// Mapeamos las dos geometrías con las que 
// hicimos los ejemplos.
Map.addLayer(poli1, {color: 'FF0000'}, 'Polígono 1');
Map.addLayer(poli2, {color: '0000FF'}, 'Polígono 2');
```
---
# Desafío 2
Verifique si la recta que pasan por los puntos **[-69.2094, -33.3251]** y **[-69.172, -33.4094]** intersecta la intersección (valga la redundancia) de las geometrías utilizadas previamente (poli1 y poli2).

--- 
# Desafío 3:
Ahora compruebe si el punto definido abajo está contenido en la geometría que resultó de la diferencia simétrica.

```javascript
var punto = ee.Geometry.Point([-69.1692, -33.3675]);
```
---
# Creación de Features

- Un Feature de Earth Engine se define como un GeoJSON Feature. 
- En pocas palabras, es un objeto con una propiedad de tipo **Geometry** y un conjunto de atributos almacenados en un diccionario (__ee.Dict({})__).

---
# Creación de Features

Entonces, necesitamos un objeto __Geometry__ y opcionalmente un diccionario con los atributos de ese Feature.

```javascript

// La geometría
var poligono = ee.Geometry.Polygon(
        [[[-79.66702667419747,-1.4023052538671108],
          [-79.67801300232247,-1.6315643269388478],
          [-79.46789947693185,-1.6398007155944714],
          [-79.46377960388497,-1.3981866110774313],
          [-79.66702667419747,-1.4023052538671108]]]);

---
# Declaración de Features

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
var dict = {distancia: ee.Number(10).add(150), 
			lugar: 'Chivilcoy'};
var featureSinGeo = ee.Feature(null, dict);
```
---
# Operaciones básicas sobre Features

Los Features tienen las mismas funcionalidades para gestionar sus geometrías que los objetos Geometry. Además, poseen otros métodos __setters & getters__ para el manejo de las propiedades.


```javascript
// Creamos un Feature y le incorporamos dos atributos
// Nombre y Altura
var feature_ejemplo = 
      ee.Feature( ee.Geometry.Point(
        [-79.3283, -1.7819]))
        .set('Nombre', 'Eldes Monte')
        .set('Altura', 100);
```

---
# Operaciones básicas sobre Features II

```
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
---


# FeatureCollection de una muestra al azar

Es posible crear un FeatureCollection a partir de generar una muestra al azar [ee.FeatureCollection.randomPoints](https://developers.google.com/earth-engine/api_docs#eefeaturecollectionrandompoints) de puntos dada una región.

```javascript
var region = ee.Geometry.Rectangle(-63.457, -25.155, -62.699, -24.714);
var randomPoints = ee.FeatureCollection.randomPoints( 
    region,
    100, // cantidad de puntos
    123); // seed

print(randomPoints)

Map.centerObject(randomPoints);

Map.addLayer(randomPoints, {}, 'Puntos al azar');
```
--- 

# Creación de FeaturesCollection desde el Mapa

Al igual que mostramos anteriormente como dibujar geometrías, es posible definir FeaturesCollection dibujando desde el mapa.

![100% center](../images/image17.png)        


---
# Creación de FeaturesCollection desde el Mapa

En la configuración podemos darle un nombre al Layer y agregar propiedades con valores por defecto y definir el color.

![](../images/features_desde_mapa.png)

---
# Creación de FeaturesCollection desde el Mapa

Podemos incorporar este FeaturesCollection desde la sección de Imports.

![](../images/image14.png)


--- 


# Operador Selection

Selección de propiedades de un Feature para generar un nuevo FeatureCollection.

**Importante:** las selecciones aplican sobre las __columnas__, es decir, sobre los atributos de una colección de features o las bandas cuando veamos colecciones de imágenes.


```javascript
var fc_seleccion = filtrados.select( 
 	['area_ha', 'class'], ['AREA','CLASE']);

print(fc_seleccion);
```
---



# Exportar como tabla de datos

Para exportar un FeatureCollection a Google Drive se requiere la instrucción Export.table.toDrive()

![](../images/image26.png)

---
# KML

```javascript
Export.table.toDrive({
          collection: <Nombre del FC>,
          description:'Una descripción de la capa para encontrarlo en Drive',
          fileFormat: 'KML o KMZ'
});
```

# CSV

```javascript
Export.table.toDrive({
    collection: <Nombre del FC>,
    description:'Una descripción de la tabla para encontrarla en Drive',
    fileFormat: 'CSV'
});
```
---
# Un ejemplo picante! :D

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
---

# Tareas de exportación
Luego de exportar se incluirá una nueva tarea que hay que poner a
correr.

![](../images/image11.png)

Dependiendo de la cantidad de features en la colección el proceso puede demorar mucho o poco y además también dependerá de la carga de trabajo de la plataforma.

![](../images/image19.png)

---


Bibliografía
============

API | Google Earth Engine API. [https://developers.google.com/earth-engine/api\_docs](https://developers.google.com/earth-engine/api_docs)

* * * * *

[[1]](#ftnt_ref1) European Petroleum Survey Group
