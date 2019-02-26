var ciudad = ee.Geometry.Point(-79.91486, -2.12678);

Map.addLayer(ciudad);

var star = ee.Date('2010-01-01');
var end = ee.Date('2010-12-31');

var guayas = ee.ImageCollection('LANDSAT/LE07/C01/T1_TOA')
.filterBounds(ciudad)
.filterDate(star,end);

var contar = guayas.size();
print('cuantas imágens hay',contar)
var mejori = ee.Image(guayas.sort('CLOUD_COVER').first());
print('la mejor es',mejori)

var fecha = mejori.get('DATE_ACQUIRED')
print('fecha tomada',fecha)

var gua = ee.Image('LANDSAT/LE07/C01/T1_TOA/LE07_011061_20001123');

Map.addLayer(gua)

var image  = ee.Image('LANDSAT/LE07/C01/T1_TOA/LE07_011061_20101002');


var nombreBandas = image.bandNames();
print('Nombre de las bandas:', nombreBandas);

var proyeccion = image.select('B1').projection();
print('La proyección es:', proyeccion)

var escala = image.select('B1').projection().nominalScale();
print('la escala es (m): ', escala)

var propiedades = image.propertyNames();
print('Las propiedades son:', propiedades);

var nubes = image.get('CLOUD_COVER');
print('las nubes :', nubes)


var sistema= image.get('system:footprint');
print('el sistema de B2 es :', sistema)


var fecha = ee.Date(image.get('system:time_start'));
print('La fecha es: ' ,fecha)







