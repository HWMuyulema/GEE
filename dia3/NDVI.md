// Obtener la imagen con menos nubes del año 2017.

var image = ee.Image(   
  l8.filterBounds(point)  
    .filterDate('2017-01-01', '2017-12-31')   
    .sort('CLOUD_COVER')  
    .first()  
);  

  

// Analizar el indice de diferencia normailizada de la vegetacion NDVI  
// Forma 1  
var nir = image.select('B5'); 
var red = image.select('B4'); 
var ndvi = nir.subtract(red).divide(nir.add(red)).rename('NDVI'); 

  

// Forma 2  

var ndvi = image.normalizedDifference(['B5', 'B4']).rename('NDVI'); 
Map.centerObject(image, 9); 
var ndviParams = {min: -1, max: 1, palette: ['blue', 'white', 'green']};  
Map.addLayer(ndvi, ndviParams, 'NDVI image'); 

//Forma 3 matemática de bandas



var ndvi = image.select('B5').subtract(image.select('B4'))  
            .divide(image.select('B5').add(image.select('B4')))    

Map.centerObject(image, 9); 
        
var ndviParams = {min: -1, max: 1, palette: ['blue', 'white', 'green']};  
Map.addLayer(ndvi, ndviParams, 'NDVI image');
