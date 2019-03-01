var landcover = ee.Image('MCD12Q1/MCD12Q1_005_2001_01_01').select('Land_Cover_Type_1');   
print(landcover)

  

Map.addLayer(landcover) 



var classes = landcover.reduceToVectors({ 
  reducer: ee.Reducer.countEvery(),   
  geometry: geometry,   
  scale: 30,  
  maxPixels: 1e8    
}); 
var result = ee.FeatureCollection(classes); 
Map.addLayer(result);
