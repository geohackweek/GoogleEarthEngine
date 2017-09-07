---
title: "Time Series"
teaching: 15
exercises: 0
questions:
- How do I get time series data?
- How can I plot data within Google Earth Engine?
- "How do I create an interactive panel "
objectives:
-
keypoints:
- "GEE has increasing functionality for making interactive plots."
- "Time series data can be extracted from Image Collections for points and regions."
- "Time series data can be extracted from Image Collections for points and regions."

---

// Set Environmental for Training Data Placement
// Nov 25 2016
// Jill Deines

// note:  modified to be more generic as a potential script in Geohackweek 2017

// User specs ---------------------------------------------------------------------------

// 1) set extent
var setExtent = ee.FeatureCollection('ft:19B25w0it5jYMBrsBpo4lIZcAYcHEcrOhFkrT5PX7');

// 2) set year
var year = 2010;

// 3) specify imagery sources
var imageFolder = 'users/jdeines/HPA/'


// End user specs -------------------------------------------------------------

// Load stuff ------------------------------------------------------------------

// CDL
var cdlName = ee.String('USDA/NASS/CDL/').cat(ee.Number(year).format());
if (year == 2005 || year == 2007){
  var cdlName = cdlName.cat('a');
}
var cdl = ee.Image(cdlName.getInfo()).clip(setExtent);

// NAIP aerial photos
var StartDate = year + '-01-01';
var EndDate = year + '-12-31';

var naip = ee.ImageCollection('USDA/NAIP/DOQQ')
    .filterBounds(setExtent)
    .filterDate(StartDate, EndDate);

// My Landsat composites --------------------

// annual max greenness images
var annualGreenest = ee.Image(ee.String(imageFolder).cat(ee.Number(year).format()).cat('_14_Ind_001').getInfo())
                    .select(['GI_max_14','EVI_max_14']).clip(setExtent);

// add satellite time series: MODIS EVI 250m 16 day -------------
var collectionModEvi = ee.ImageCollection('MODIS/MOD13Q1')
    .filterDate(StartDate,EndDate)
    .filterBounds(setExtent)
    .select("EVI");

// add satellite time series: MODIS NDVI 250m 16 day -------------
var collectionModNDVI = ee.ImageCollection('MODIS/MOD13Q1')
    .filterDate(StartDate,EndDate)
    .filterBounds(setExtent)
    .select("NDVI");


// Visualize ----------------------------------------------------------------------------------
Map.centerObject(setExtent, 8);

// A nice EVI palette
var palette = [
  'FFFFFF', 'CE7E45', 'DF923D', 'F1B555', 'FCD163', '99B718',
  '74A901', '66A000', '529400', '3E8601', '207401', '056201',
  '004C00', '023B01', '012E01', '011D01', '011301'];

// greenest images
Map.addLayer(annualGreenest.clip(setExtent).select('GI_max_14'),
    {min:.75, max: 15, palette: palette}, 'GI - annual');

// cdl and naip
Map.addLayer(cdl, {}, 'cdl', false);
Map.addLayer(naip, {}, 'naip', false);


// Create User Interface portion --------------------------------------------------
// Create a panel to hold our widgets.
var panel = ui.Panel();
panel.style().set('width', '300px');

// Create an intro panel with labels.
var intro = ui.Panel([
  ui.Label({
    value: 'Two Chart Inspector',
    style: {fontSize: '20px', fontWeight: 'bold'}
  }),
  ui.Label('Click a point on the map to inspect.')
]);
panel.add(intro);

// panels to hold lon/lat values
var lon = ui.Label();
var lat = ui.Label();
panel.add(ui.Panel([lon, lat], ui.Panel.Layout.flow('horizontal')));

// Register a callback on the default map to be invoked when the map is clicked
Map.onClick(function(coords) {
  // Update the lon/lat panel with values from the click event.
  lon.setValue('lon: ' + coords.lon.toFixed(2)),
  lat.setValue('lat: ' + coords.lat.toFixed(2));
  var point = ee.Geometry.Point(coords.lon, coords.lat);


  // Create an MODIS EVI chart.
  var eviChart = ui.Chart.image.series(collectionModEvi, point, ee.Reducer.mean(), 250);
  eviChart.setOptions({
    title: 'MODIS EVI',
    vAxis: {title: 'EVI', maxValue: 9000},
    hAxis: {title: 'date', format: 'MM-yy', gridlines: {count: 7}},
  });
  panel.widgets().set(2, eviChart);

  // Create an MODIS NDVI chart.
  var ndviChart = ui.Chart.image.series(collectionModNDVI, point, ee.Reducer.mean(), 250);
  ndviChart.setOptions({
    title: 'MODIS NDVI',
    vAxis: {title: 'NDVI', maxValue: 9000},
    hAxis: {title: 'date', format: 'MM-yy', gridlines: {count: 7}},
  });
  panel.widgets().set(3, ndviChart);
});

Map.style().set('cursor', 'crosshair');

// Add the panel to the ui.root.
ui.root.insert(0, panel);
