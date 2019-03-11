var geometry = /* color: #f0f0f0 */ee.Geometry.Point([-79.420, -1.8155]);

// ir a VV collection.
var collectionVV = ee.ImageCollection('COPERNICUS/S1_GRD')
    .filter(ee.Filter.eq('instrumentMode', 'IW'))
    .filter(ee.Filter.listContains('transmitterReceiverPolarisation', 'VV'))
    .filter(ee.Filter.eq('orbitProperties_pass', 'DESCENDING'))
    .select(['VV']);

// Create a 3 band stack by selecting from different periods (months)
var im1 = ee.Image(collectionVV.filterDate('2017-12-01', '2018-01-30').mean());
var im2 = ee.Image(collectionVV.filterDate('2018-02-01', '2018-02-28').mean());
var im3 = ee.Image(collectionVV.filterDate('2018-03-01', '2018-04-30').mean());

Map.centerObject(geometry, 13);
Map.addLayer(im1.addBands(im2).addBands(im3), {min: -25, max: 4}, 'VV stack');
