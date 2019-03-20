# Inundaciones
[Ejercicios inundación Eric](https://code.earthengine.google.com/c08a5d0cf27c5a277401686e526c5ada)

//Consultar imagenes disponibles
  var collectionVV = ee.ImageCollection('COPERNICUS/S1_GRD')
  .filter(ee.Filter.eq('instrumentMode', 'IW'))
  .filter(ee.Filter.listContains('transmitterReceiverPolarisation', 'VV'))
  .filterDate('2019-01-07', '2019-01-17')
  .select(['VV'])
  .filterBounds(zona_estudio)
    
  print(collectionVV)
  
//Visualizar bandas VV y VH  
  var imgVV =ee.Image('COPERNICUS/S1_GRD/S1A_IW_GRDH_1SDV_20190115T110104_20190115T110133_025487_02D340_2197')  
  .select(['VV']) 
  
  var imgVH =ee.Image('COPERNICUS/S1_GRD/S1A_IW_GRDH_1SDV_20190115T110104_20190115T110133_025487_02D340_2197')  
  .select(['VH']) 
  
  Map.addLayer(imgVV, visVV, 'imgVV') 
  Map.addLayer(imgVH, visVH, 'imgVH') 
    
//Crear histograma de valores de píxeles de cuerpos de agua seleccionados

  var hist = ui.Chart.image.histogram({ 
  image: imgVV, 
  region: geometry  
  })  
  
  print(hist) 
   
//Visualizar y comparar mascaras de inundaciones utilizando diferentes umbrales

  Map.addLayer(imgVV.updateMask(imgVV.lte(-15)),{},'-15') 
  Map.addLayer(imgVV.updateMask(imgVV.lte(-16)),{},'-16') 

//Convertir mascara de inundaciones a vector  
  var mascara = imgVV.updateMask(imgVV.lte(-15))  
  
  var inund_vector = mascara.eq(1).reduceToVectors  
  ({  
  geometry:zona_estudio,  
  scale:10 ,  
  crs : 'EPSG:32717', 
  geometryType:'polygon',   
  eightConnected:false,   
  bestEffort:false,  
  maxPixels:1e15})  

  Map.addLayer(inund_vector,{},'inund_vector')
  
  

//Visualizar y exportar vector aplicando una unidad mínima en función del nro. de píxeles por polígono

  var inund_vector_UM = inund_vector.filter(ee.Filter.gte('count', 50)) 
  
  Map.addLayer(inund_vector_UM,{},'vector_UM_pixeles')    

//Exportar vector (shapefile) 

Export.table.toDrive({  
  collection: inund_vector_UM,  
  fileFormat: 'SHP' 
})  


