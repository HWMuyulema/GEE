[Luego de haber entendido los conseptos básicos]( https://github.com/HWMuyulema/GEE/blob/master/conceptos_basicos.md 'Nada es  dificil' )

## Vamos a escribir 

- Declaramos una ubicación

var ciudad = ee.Geometry.Point();

Map.addLayer(ciudad);

- Definimos fechas

var star = ee.Date('');
var end = ee.Date('');

var guayas = ee.ImageCollection('')
.filterBounds(ciudad)
.filterDate(star,end);

- Conteo de imágenes

var contar = guayas.size();
print('cuantas imágenes hay',contar)

- Cantidad de nubes

var cantidadaDeNubes = ee.Image(guayas.sort('CLOUD_COVER').first());
print('la mejor es',mejori)

- Cual es la imagen con menos cantidad de nubes

var fecha = mejori.get('DATE_ACQUIRED')
print('fecha tomada',cantidadaDeNubes)

- Imagen

var imagen = ee.Image('id:una sola imagen');

Map.addLayer(imagen)

- Nombre de las bandas

var nombreBandas = imagen.bandNames();
print('Nombre de las bandas:', nombreBandas);

- SRC

var proyeccion = imagen.select('B1').projection();
print('La proyección es:', proyeccion)

- Px

var escala = imagen.select('B1').projection().nominalScale();
print('la escala es (m): ', escala)

-Propiedades de la imagen

var propiedades = imagen.propertyNames();
print('Las propiedades son:', propiedades);

- Propiedad específica

var nubes = imagen.get('CLOUD_COVER');
print('las nubes :', nubes)


var sistema= imagen.get('system:footprint');
print('el sistema de B2 es :', sistema)


var fecha = ee.Date(imagen.get('system:time_start'));
print('La fecha es: ' ,fecha)







