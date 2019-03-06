# Clasificación supervisada


La [clasificación supervisada y no supervisada](https://acolita.com/clasificacion-supervisada-no-supervisada-en-arcgis/) permite explorar diferentes tipos de atributos o clases por medio del análisis 

estadístico multivariado, este proceso identifica los valores de cada píxel de una o varias bandas de una imagen ráster, 

crea y evalua las clases o clúster (firmas), finalmente reclasifica de acuerdo a las probabilidades de cada clase. 

Este procedimiento es muy práctico para crear de forma automática un mapa de uso del suelo o de cobertura vegetal 

Utilizaremos el método jerraquico ## [CART](https://es.slideshare.net/techi322/algoritmos-de-clasificacin),arboles de clasificación y regresión.


# GEE



var image = ee.Image(l8.filterBounds(roi)   
    .filterDate('2016-01-01', '2018-12-31') 
    .sort('CLOUD_COVER')    
    .first());  
Map.addLayer(image, {bands: ['B5', 'B4', 'B3'], max: 0.3}, 'image');    


var newfc = urbano.merge(cultivo).merge(agua);  
// print(newfc);    

var bands = ['B2', 'B3', 'B4', 'B5', 'B6', 'B7'];   

var training = image.select(bands).sampleRegions({  
  collection: newfc,    
  properties: ['landcover'],     
  scale: 30 
});     

// print(training)  



var classifier = ee.Classifier.cart().train({   
  features: training,    
  classProperty: 'landcover',   
  inputProperties: bands    
}); 
print(classifier.explain());    



var classified = image.select(bands).classify(classifier);  
Map.addLayer(classified, {min: 0, max: 2, palette: ['yellow', 'blue', 'red']}); 





var corte = classified.clip(guayas) 
Map.addLayer(corte, {min: 0, max: 2, palette: ['yellow', 'blue', 'red']}, 'clasificacion guayas');  
print(corte)    



Export.image.toDrive({  
  image: corte, 
  description: 'clasificacionGuayas',   
  scale: 30,    
  region: guayas,   
  fileFormat: 'GeoTIFF',    
  maxPixels: 1e12,  
  crs : 'EPSG:32717'    
  })


Link de hoy: [Github WHMuyulema](https://code.earthengine.google.com/0f6ce47afcc96cf638ef365f99349861)
