var imageCollection : ImageCollection"Sentinel-1 SAR GRD...  
Var  table : table (subir la tabla propuesta)  
var Geometry ; Polygon (hacer un poligono del área propuesta)  

----------------

var s1 = ee.ImageCollection('COPERNICUS/S1_GRD')    
    .filterDate('2018-11-30','2019-04-30')  
    .filterBounds(pt)   
    .select('VV')   
    
    
    
    Map.addLayer(s1,{},'',0)
    
    


var radar1 = ee.Image('COPERNICUS/S1_GRD/S1A_IW_GRDH_1SDV_20181210T110105_20181210T110134_024962_02C050_F058')  
Map.addLayer(radar1,{},'',0)    



Map.addLayer(table2,{},'',0)    
var newb = radar1.addBands(radar1.select('VH').multiply(radar1.select('VH')).select([0],['2VH']))   



var newb2 = newb.addBands(newb.select('VV').divide(newb.select('VH').divide(100)).select([0],['div']))  
print(newb2)    



Map.addLayer(newb2,{},'',0) 



Map.addLayer(table,{},'',0) 




Map.addLayer(table) 



// es otra foram de declarar una imagen vacia
// Convierte el valor de entrada en un entero de 8 bits sin signo   




var vacio = ee.Image().byte()   


var colorlinea = vacio.paint({  
 featureCollection:table,   
color:1,    
 width:3    
 }) 
 
 Map.addLayer(colorlinea,{palette: 'ff0000'}, 'linea banano',0)     
 
 
 Map.addLayer(table2,{},'',0)   
 
 
 var vacio2 = ee.Image().byte() 
 
 
 var colorlinea2 = vacio2.paint({   
   featureCollection:table2,    
   color:1, 
   width:2  
 }) 
 
Map.addLayer(colorlinea2,{palette: '00ff00'}, 'linea banano2',0)    



print(newb2)    


Map.addLayer(newb2,{bands:['VV','2VH','div'],min:[-19,-317,6], max:[6,410,90]},'teos12',0)  


var corte = newb2.clip(geometry)    



 Map.addLayer(corte,{bands:['VV','2VH','div'],min:[-19,-317,6], max:[6,410,90]},'teoscorte12',0)    
 

var banano = newb2.gte(-4)  


 //print(banano)    
 

 Map.addLayer(banano,{},'teo')  
 

var todospx = corte.reduceRegion({  
reducer:ee.Reducer.sum(),   
geometry: table,    
scale:10,   
maxPixels:1e10  
})  



// print(todospx)   


var zonas = corte.gt(-12).add(corte.eq(-4)) 
zonas = zonas.updateMask(zonas.neq(0))  
print('estas son las zonas',zonas)  



Map.addLayer(zonas,{},'zonas de corte',0)   

var vectors = zonas.addBands(newb2).reduceToVectors({   
geometry: table,    
crs: corte.select('VV').projection(),// código sofware libre de WGS84 UTM17S y geograficas 4216 
scale: 10,  
geometryType: 'polygon',    
eightConnected: false,  
labelProperty: 'zonas', 
reducer: ee.Reducer.mean(), 
maxPixels: 1e11 
}); 



print('vectores',vectors)   

// desplegar los umbrales.  

Map.addLayer(zonas, {min: 1, max: 2}, 'raster');    

// Hacer una imagen para vectores, y añadirla al mapa   

var display = ee.Image(0).updateMask(0).paint(vectors, '000000', 2);    
Map.addLayer(display, {}, 'vectors');   



Export.table.toDrive({  
collection: vectors,    
description: "vectores",    
fileFormat:"KML"    
})  




Export.image.toDrive({  
  image: corte, 
  description: 'radar', 
  scale: 100,   
  maxPixels: 1e9,   
  region: geometry  
}); 




[Ejercicio radar](https://code.earthengine.google.com/8e4f9e88cb0de6965ae57f346baf89c0)
