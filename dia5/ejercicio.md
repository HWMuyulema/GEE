var imageCollection : ImageCollection"Sentinel-1 SAR GRD...  
Var  table : table (subir la tabla propuesta)  
var Geometry ; Polygon (hacer un poligono del área propuesta)  

----------------
```
var teos1 = ee.ImageCollection('COPERNICUS/S1_GRD') | 
    .filterDate('2018-12-01', '2018-12-14') | 
    .filterMetadata('instrumentMode','equals','IW') | 
    .filterMetadata('orbitProperties_pass','equals','DESCENDING')|  
    .filter(ee.Filter.listContains('transmitterReceiverPolarisation', 'VV'))    
    .select(['VV']);    

    .filterBounds(geometry)  
    
   // Create a 3 band stack by selecting from different periods (months)
   
// var im1 = ee.Image(collectionVV.filterDate('2018-11-01', '2019-01-31').mean());

// var im2 = ee.Image(collectionVV.filterDate('2016-02-01', '2016-03-15').mean());

// var im3 = ee.Image(collectionVV.filterDate('2016-03-16', '2016-04-30').mean());


// Map.centerObject(geometry, 15);

// Map.addLayer(im1.addBands(im2).addBands(im3), {min: -16, max: -4}, 'VV stack');


    
    //Map.setCenter(-79,-1,8)  
    //Map.addLayer(teos1)  
    
    var teos12 = ee.Image('COPERNICUS/S1_GRD/S1A_IW_GRDH_1SDV_20181210T110105_20181210T110134_024962_02C050_F058')  
    
    var newb = teos12.addBands(teos12.select('VH').multiply(teos12.select('VH')).select([0],['2VH']))  
   
   var newb2 = newb.addBands(newb.select('VV').divide(newb.select('VH').divide(100)).select([0],['div']))  
   print(newb2)  
   
   Map.addLayer(newb2,{bands:['VV','2VH','div'],min:[-19,-317,6], max:[6,675,90]},'teos12',0)  
   
   var corte = newb2.clip(geometry)  
   Map.addLayer(corte,{bands:['VV','2VH','div'],min:[-19,-317,6], max:[6,675,90]},'teoscorte12')  
   
   
   var banano = newb2.gte(-5)  
  
   print(banano)  
  
  Map.addLayer(banano,{},'teo',0)  
  
  var todospx = corte.reduceRegion({  
    reducer:ee.Reducer.sum(),  
    geometry: table,  
    scale:10,  
    maxPixels:1e10  
  })
  
  
  print(todospx)
  
  var zonas = corte.gt(-12).add(corte.eq(-4))  
  zonas = zonas.updateMask(zonas.neq(0))  
  print('estas son las zonas',zonas)  
  
  
Map.addLayer(zonas,{},'',0)
  
  var vectors = zonas.addBands(newb2).reduceToVectors({  
  geometry: table,  
  crs: corte.select('VV').projection(),  
  scale: 10,  
  geometryType: 'polygon',  
  eightConnected: false,  
  labelProperty: 'zonas',  
  reducer: ee.Reducer.mean(),  
  maxPixels: 1e11  
});

print('vectores',vectors)

// desplegar los umbrales.  
//Map.setCenter(139.6225, 35.712, 9);  
Map.addLayer(zonas, {min: 1, max: 2}, 'raster');  
 
// Hacer  una imagen para vectores, y añadirla al mapa

var display = ee.Image(0).updateMask(0).paint(vectors, '000000', 2);  
Map.addLayer(display, {}, 'vectors');  

Export.table.toDrive({  
  collection: vectors,   
  description: "vectores",   
    fileFormat:"KML"  
})


[Ejercicio radar](https://code.earthengine.google.com/c79b38cf504a0030170775d2bb547aa4)
