---
title: "Accessing Satellite Imagery"
teaching: 5
exercises: 10
questions:
- How do I acquire satellite imagery?
- How can I make image mosaics?
- How can I use my own vector data?
- How do I export images?
objectives:
- Learn how to access images based on filter criteria
- Perform basic image manipulation, mosaicking, and visualization
- Understand how to map functions across image collections
- Learn how to export images for later use
keypoints:
- Satellite data can be loaded by specific image names or by loading a full image collection and filtering based on criteria such as time period and location
- Images can be manipulated similar to raster math, and new bands can be calculated by mapping functions over image collections
- Image composites can be generated quite quickly 
- Images can be exported for future use within GEE or for outside software
---

## Load a Satellite Image
In Google Earth Engine (GEE), raster data are called `Images`. Each image has 1 or more named bands as well as metadata stored as properties. More information on Images can be found [here in the GEE Developer's Guide](https://developers.google.com/earth-engine/image_overview). 

Here, we will load a specific image from the Landsat 8 Surface Reflectance product ([described here in the data catalog](https://code.earthengine.google.com/dataset/LANDSAT/LC8_SR)). These satellite images have already been processed to surface reflectance values using standard algorithms by the USGS and come with a cloud mask band ('cfmask').

Note that each individual image in GEE has an Image ID. We will load an image over Seattle from July 28, 2016. The image ID is LC80460272016209. Landsat image ID's follow this pattern: **image collection + path number + row number + year + julian day**, or in this case, LC8 + 046 + 027 + 2016 + 209. Images from other data products have similar logical naming patterns.

We can look at the image we've loaded in two ways:

* print the image, which displays image metadata in the console (upper right panel)
* add the image to the map panel, specifying how we'd like to visualize it

{% highlight javascript %}
// load an image by name (collection Id + image id)
var image = ee.Image('LANDSAT/LC8_SR/LC80460272016209')

// view the image metadata in the Console
print(image);
{% endhighlight %}

<br>
<img src="../fig/03_printImage.png" border = "10">
<br><br>

{% highlight javascript %}
// add the image to the map, setting the bands to use for RGB
Map.centerObject(image, 8); // specify map location and zoom level, if desired
Map.addLayer(image, {bands: ['B4', 'B3', 'B2'], min: 0, max: 2000}, 'L8 SR');
{% endhighlight %}

Note the arguments for Map.addLayer() are Map.addLayer(eeObject, dictionary of visualization parameters, layer name...). These are described in the `Map` section of the Docs panel.

<br>
<img src="../fig/03_tccSeattle.png" border = "10">
<br><br>

### Manipulate Image Data: Mask clouds
The `Docs` tab in the upper left panel provides a description of all the available functions for image manipulation under the `ee.Image` dropdown menu. There are a lot, including mathematical and boolean operators, convolutions and focal statistics, and spectral transformations and analyses of spatial texture. Browse the list, or read about general operations available in the [GEE Developer's Guide "Image Overview"" section](https://developers.google.com/earth-engine/image_overview).

Here, we'll make use of the `cfmask` cloud band provided with the SR products to mask pixels with clouds and cloud shadows. We will mask pixels in the image based on the value of cfmask.

{% highlight javascript %}
// mask pixels with clouds and cloud shadows
// surface reflectance products come with a 'cfmask' layer
// 0 = clear, 1 = water, 2 = cloud shadows, 3 = snow, 4 = clouds
var cfmask = image.select('cfmask');  // extract the 'cfmask' band   
// keep pixels not equal (neq) to 2 or 4
var imageMasked = image.updateMask(cfmask.neq(2)).updateMask(cfmask.neq(4));  

// visualize the masked image
Map.addLayer(imageMasked, {bands: ['B4', 'B3', 'B2'], min: 0, max: 2000}, 'clouds masked');
{% endhighlight %}

<br>
<img src="../fig/03_tccSeattleMasked.png" border = "10">
<br><br>

## Mosaicking Multiple Images: Image Collections
A stack or time series of images are called `Image Collections`. Each data source available on GEE has it's own Image Collection and ID (for example, the [Landsat 5 SR collection](https://code.earthengine.google.com/dataset/LANDSAT/LT5_SR), the [GRIDMET meteorological data collection](https://code.earthengine.google.com/dataset/IDAHO_EPSCOR/GRIDMET)). You can also create image collections from individual images or merge existing collections. More information on Image Collections can be found [here in the GEE Developer's Guide](https://developers.google.com/earth-engine/ic_creating).

In order to generate images that cover large spatial areas and to fill in image gaps due to clouds, etc, we can load a full `ImageCollection` but filter the collection to return only the time periods or spatial locations that are of interest. There are shortcut filters for those commonly used (imageCollection.filterDate(), imageCollection.filterBounds()...), but any filter in the `ee.Filter()` section of the Docs tab can be used. Read more about [filters on the Developer's Guide](https://developers.google.com/earth-engine/ic_filtering).

### Load Vector Boundary
We'll work on making a composite satellite image for the state of Washington. The easiest way to filter for an irregular location without having to identify the paths and rows of teh satellite image tiles is to use a vector polygon.

There are three ways to use vector data in GEE:

* [Upload a shapefile](https://developers.google.com/earth-engine/importing) to your personal **Asset** folder in the top left panel. You can set sharing permissions on these as needed. 
* Import an existing [Google Fusion Table](https://support.google.com/fusiontables#topic=1652595), or [create your own](https://fusiontables.google.com/data?dsrcid=implicit) fusion table from a KML in WGS84.  Each fusion table has a unique Id (File > About this table) that can be used to load it into GEE. You also need to set sharing permissions similar to other items in your Google Drive if you want others to be able to access your fusion table. GEE only recently added the Asset option, so you may see folks still using fusion tables in the forums, etc. If you have the choice, I'd use an asset. We use a fusion table in the [Spatial and Temporal Reducers Module](https://geohackweek.github.io/GoogleEarthEngine/05-reducers/).
* Manually draw points, lines, and polygons using the geometry tools in the code editor. We do this in the [Classify Imagery Module](https://geohackweek.github.io/GoogleEarthEngine/04-classify-imagery/).

Here, we will use a vector asset loaded to your instructor's personal asset folder and made publicly available through the sharing menu. The file location is 'users/jdeines/vector/examples/WA'. When you upload a vector file (called a table) to your asset folder using the "New" button under the **Assets tab** in the upper left panel of the code editor, it will be stored in 'users/yourUserName/filename' unless you create new folders within your Assets space. For more on importing vector files, see the [Developer's Guide section on Importing Table Data](https://developers.google.com/earth-engine/importing). Tip: when uploading a shapefile, you need to select all associated files (.dbf, .shx, .prf, etc) or upload a zipped file containing only one shapefile.

In order to load a vector file from your Assets into your workspace, we need to use the "filepath" and cast it to a `ee.FeatureCollection` data type. Read more here under ["Managing Assets" in the Developer's Guide](https://developers.google.com/earth-engine/asset_manager#importing-assets-to-your-script).

{% highlight javascript %}
// load a polygon boundary (a shapefile previously uploaded to a user 'Assets' folder)
var boundary = ee.FeatureCollection('users/jdeines/vector/examples/WA');
print(boundary);
Map.addLayer(boundary, {}, 'WA');
{% endhighlight %}

<br>
<img src="../fig/03_waBound.png" border = "10">
<br><br>

### Load an image collection based on filter criteria
Here, we are selecting all imagery in the [Landsat 8 surface reflection collection](https://code.earthengine.google.com/dataset/LANDSAT/LC8_SR) (image collection IDs are found in the "Search" toolbar at the top of the code editor or through searcing the [data archive](https://code.earthengine.google.com/datasets/)) for the year 2016 that overlies the Washington state vector polygon we loaded above as the variable "boundary".

{% highlight javascript %}
var cWA = ee.ImageCollection('LANDSAT/LC8_SR')
          .filterBounds(boundary)
          .filterDate('2016-01-01', '2016-12-31');
print(cWA);
{% endhighlight %}

### Mapping functions: mask clouds, calculate new band

#### Mask clouds over an image collection

{% highlight javascript %}
// create function to mask clouds, cloud shadows, snow
var maskClouds = function(img){
  var cfmask = img.select('cfmask');    
  return img.updateMask(cfmask.eq(0));   // keep clear (0) pixels
};

// use "map" to apply the function to each image in the collection
var cMasked = cWA.map(maskClouds);
{% endhighlight %}

#### Calculate NDVI and Add Band to Images

{% highlight javascript %}
// Calculate NDVI and add as a new band
// use "map" to apply an anonymous function to each image
var cNDVI = cMasked.map(function(img){
  // add a NDVI band to each image
  return img.addBands(img.normalizedDifference(['B5','B4']).rename('NDVI'));
});
{% endhighlight %}

### Create an Image Composite from Collection

{% highlight javascript %}
// Make a "greenest pixel" composite for WA state
var greenest = cNDVI.qualityMosaic('NDVI').clip(boundary);
print(greenest);

// Visualize NDVI
var ndviPalette = ['FFFFFF', 'CE7E45', 'DF923D', 'F1B555', 'FCD163', '99B718',
               '74A901', '66A000', '529400', '3E8601', '207401', '056201',
               '004C00', '023B01', '012E01', '011D01', '011301'];
Map.addLayer(greenest.select('NDVI'), 
            {min:0.1, max: 1, palette: ndviPalette}, 'ndvi');
{% endhighlight %}

<br>
<img src="../fig/03_waNDVI.png" border = "10">
<br><br>


{% highlight javascript %}
// Visualize true color composite
Map.addLayer(greenest, {bands: ['B4', 'B3', 'B2'], min: 0, max: 2000}, 'WA tcc', false);
{% endhighlight %}

<br>
<img src="../fig/03_waTCC.png" border = "10">
<br><br>

## Exporting Composite Images

{% highlight javascript %}
// select only the ndvi band
var ndvi = greenest.select('NDVI');

// Export to your folder on Google Drive 
// (note: need to hit 'Run' in the task tab in upper right panel) 
Export.image.toDrive({
  image: ndvi,
  description: 'Washington_NDVI_2016',
  scale: 30,
  region: boundary.geometry().bounds(), // .geometry().bounds() needed for multipolygon
  crs: 'EPSG:5070',
  folder: 'GEE_geohackweek',
  maxPixels: 2000000000
});

// Export to your ASSET folder 
// (note: need to hit 'Run' in the task tab in upper right panel) 
Export.image.toAsset({
  image: ndvi,
  description: 'Washington_NDVI_2016_asset',
  assetId: 'users/yourname/2016_WA_ndvi',
  scale: 30,
  region: boundary.geometry().bounds(),
  pyramidingPolicy: {'.default':'mean'}, // use {'.default':'sample'} for discrete data
  maxPixels: 2000000000
});
{% endhighlight %}

