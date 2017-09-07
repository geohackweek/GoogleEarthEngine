---
title: "Time Series"
teaching: 15
exercises: 0
questions:
- How do I create a time series for a given location?
- How can I plot that time series within Google Earth Engine?
- "How do I make an interactive panel plot?"
objectives:
- "Load high resolution crop imagery."
- "Dynamically select lat/longs for creating time series plots."
- "Create a time series of NDVI and EVI for a selected point."
keypoints:
- "GEE has increasing functionality for making interactive plots."
- "Time series data can be extracted from Image Collections for points and regions."
- "Time series data can be extracted from Image Collections for points and regions."

---

## Overview

This code allows users to dynamically generate time series plots for from points that are dynamically chosen on a map on the fly. The time series show the 16 day composites of Normalized Difference Vegetation Index and Enhanced Vegetation Index at 250 m resolution. These indices are derived from MODIS.


## Define User Specifications

This script is structured to make it easy for the user to select different images, dates and regions. For this exercise, we are going to leave the parameters as they are to set the extent as a study area in the Midwest. We are going to define the year that we want and also the imagery source folder.


{% highlight javascript %}

// User specs ---------------------------------------------------------------------------

// 1) set extent
// load WBD dataset & select watershed of interest
var WBD = ee.FeatureCollection("USGS/WBD/2017/HUC06");
var setExtent = WBD.filterMetadata('name', 'equals', 'Republican');

// 2) set year
var year = 2010;

// 3) specify imagery sources
var imageFolder = 'users/jdeines/HPA/'

// End user specs -------------------------------------------------------------
{% endhighlight %}
<br>

## Load High Resolution Crop Imagery

Here we are loading three different types of high resolution crop imagery. We are calling the years and folder names we specified at the beginning of the script. The first two datasets are already in Earth Engine. The third dataset is an Greenness index calculated from Landsat imagery. Instead of calculating the GI on the fly in this code, Jill pre-computed the index, exported the rasters and then is calling the pre-made raster. Practices like this can help speed up your code.

1. Cropland Data Layer (CDL) from the USDA National Agriculture Statistics Service at 30 meters resolution. This is generated annually. Each pixel is assigned a value that corresponds to a specific crop.

2. The National Agricultural Imagery Program (NAIP) aerial imagery from the USDA. This imagery has a 1 meter (!) ground sampling distance. This is a three band raster used to depict natural color imagery (RGB).

3.  A derived Greenness Index derived from Landsat imagery (30 m) specific to the study area. This index is computed by taking a composite of the greenest pixel, defined as the pixel with the highest NDVI, for a given time period.

{% highlight javascript %}
// Load imagery ------------------------------------------------------------------

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

// Derived Landsat Composites --------------------

// annual max greenness images
var annualGreenest = ee.Image(ee.String(imageFolder).cat(ee.Number(year)
  .format()).cat('_14_Ind_001').getInfo())
  .select(['GI_max_14','EVI_max_14']).clip(setExtent);

{% endhighlight %}

## Load MODIS derived products for NDVI and EVI

NDVI and EVI are two different vegetation indices that can be calculated from red and near-infrared bands. Here we are using a derived MODIS 16 day composite product that has pre-computed bands for NDVI and EVI. You could also compute them yourself using the  `normalizedDifference` function. Again we will filter the collection to the dates, bands and region of interest.

{% highlight javascript %}

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
{% endhighlight %}

{% highlight javascript %}

## Visualize the High Resolution imagery

Here we load the CDL, NAIP and Annual Greenest Pixel composites.
 - *Time Saving Tip:* If you are using a public dataset, often you can find nice palettes by sifting through forum posts.
 - *Tip:* Hexadecimal browser color picker plug-ins are helpful for figuring out which colors correspond to which codes.
 - *Tip:*: Use the `false` argument if you want to load a layer to the map but NOT have it turned on each time you run the code.

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
{% endhighlight %}

{% highlight javascript %}

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
  {% endhighlight %}

  {% highlight javascript %}

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
{% endhighlight %}
