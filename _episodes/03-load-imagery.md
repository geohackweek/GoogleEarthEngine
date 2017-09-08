---
title: "Accessing Satellite Imagery"
teaching: 5
exercises: 20
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

## Importing & Exporting Your Own Assets

#### Importing Images and Tables
Besides using all of Google's amazing archives, users can also import their own data as either images (rasters) or tables (vectors). The **Assets** tab on the left is where you can import, share and manage these own assets. You can upload images or tables (vector data) here.

When posting on the forum, make sure you check the "Anyone Can Read" box on the sharing assets pop-out so folks on the forum can all run your code. If you have a private asset and you don't make it public, other people will not be able to run your code. If you don't want to share your private data, you can create a dummy example to share on the forums using hand drawn points or polygons.

<br>
<img src="../fig/02_sharingassets.png" border = "10">
<br><br>

For an example script that uses imported data, see [Episode XX](). For detailed instructions from Google on uploading, sharing and managing assets, see the [Assets Manager page](https://developers.google.com/earth-engine/asset_manager) on the GEE website .

#### Exporting & the Tasks Tab
Instead of printing to the console, for larger tasks you may want to just export the outputs to your Google Drive or Cloud Storage using the `Export` functions in your code. When run, these generate a new task in the `Task` tab in the upper right panel. You will need to then "Run" this task in order to actually start the export. Once you start an export task, you will be prompted to enter details about the resolution, size, format and destination if you did not include this in your code. You can hover your mouse over the task and click the "?" icon to see the status and also to get the task number. If your task isn't executing, you can share this number as a reference on the developers forum.

For detailed instructions on exports, see the [Exporting Data page](https://developers.google.com/earth-engine/exporting) on the GEE website. We will also have small export examples in subsequent modules of this tutorial.
<br>
<br>


*Note: If you do not have access to the shared code repository for this tutorial, a static version of the full script used in this module can be found here:
[(https://code.earthengine.google.com/37ca6df552bd8c19488273ff9d4f444b)](https://code.earthengine.google.com/37ca6df552bd8c19488273ff9d4f444b)


### Manipulate Image Data: Mask clouds
The `Docs` tab in the upper left panel provides a description of all the available functions for image manipulation under the `ee.Image` dropdown menu. There are a lot, including mathematical and boolean operators, convolutions and focal statistics, and spectral transformations and analyses of spatial texture. Browse the list, or read about general operations available in the [GEE Developer's Guide "Image Overview"" section](https://developers.google.com/earth-engine/image_overview).

Here, we'll make use of the `cfmask` cloud band provided with the SR products to mask pixels with clouds and cloud shadows. We will mask pixels in the image based on the value of cfmask.

{% highlight javascript %}
// mask pixels with clouds and cloud shadows
// surface reflectance products come with a 'cfmask' layer
// 0 = clear, 1 = water, 2 = cloud shadows, 3 = snow, 4 = clouds

// extract the 'cfmask' band, and use this band to mask all image bands  
var cfmask = image.select('cfmask');  
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
We'll work on making a composite satellite image for the state of Washington. The easiest way to filter for an irregular location without having to identify the paths and rows of the satellite image tiles is to use a vector polygon.

There are three ways to use vector data in GEE:

* [Upload a shapefile](https://developers.google.com/earth-engine/importing) to your personal **Asset** folder in the top left panel. You can set sharing permissions on these as needed.
* Import an existing [Google Fusion Table](https://support.google.com/fusiontables#topic=1652595), or [create your own](https://fusiontables.google.com/data?dsrcid=implicit) fusion table from a KML in WGS84.  Each fusion table has a unique Id (File > About this table) that can be used to load it into GEE. You also need to set sharing permissions similar to other items in your Google Drive if you want others to be able to access your fusion table. GEE only recently added the Asset option, so you may see folks still using fusion tables in the forums, etc. If you have the choice, I'd use an asset. We use a fusion table in the [Spatial and Temporal Reducers Module](https://geohackweek.github.io/GoogleEarthEngine/04-reducers/).
* Manually draw points, lines, and polygons using the geometry tools in the code editor. We do this in the [Classify Imagery Module](https://geohackweek.github.io/GoogleEarthEngine/05-classify-imagery/).

Here, we will use a vector asset loaded to the instructor's personal asset folder and made publicly available through the sharing menu. The file location is 'users/jdeines/vector/examples/WA'. When you upload a vector file (called a table) to your asset folder using the "New" button under the **Assets tab** in the upper left panel of the code editor, it will be stored in 'users/yourUserName/filename' unless you create new folders within your Assets space. For more on importing vector files, see the [Developer's Guide section on Importing Table Data](https://developers.google.com/earth-engine/importing). **Tip**: when uploading a shapefile, you need to select all associated files (.dbf, .shx, .prf, etc) or upload a zipped file containing only one shapefile.

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
// load all Landsat 8 SR image within polygon boundary for 2016
var cWA = ee.ImageCollection('LANDSAT/LC8_SR')
          .filterBounds(boundary)
          .filterDate('2016-01-01', '2016-12-31');
print(cWA);
{% endhighlight %}

### Mapping functions: mask clouds, calculate new band
As you can see by browsing the `Docs` tab in the upper left panel of the code editor, there are GEE functions specific to both the `Image` and `ImageCollection` data types. Often, we want to use a function specific to images on all images in an image collection. To do so, we need to essentially "loop" through each image in the image collection. In GEE, "loops" are accomplished with the .map() function. **Avoid actual loops at all costs.** Using imageCollection.map() sends the operation to Google's servers for distributed runs. Loops implemented in typical JavaScript fashion bring the operation into the browser, and won't work.

More information about mapping functions over image collections can be found [here in the Developer's Guide](https://developers.google.com/earth-engine/ic_mapping).

The .map() concept applies to `featureCollections` as well - to apply a function to each feature in a collection, we map that function across the featureCollection with featureCollection.map(). See ["Mapping over a Feature Collection"](https://developers.google.com/earth-engine/feature_collection_mapping) in the Developer's Guide.

#### Mask clouds over an image collection
Here, we explicitly define a new function called "maskClouds" and apply it to each image in the imageCollection by using `imageCollection.map()`. Functions need to explicitly **return** the final output.

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
You can also map **anonymous functions** over image collections instead of explicitly defining new functions. In the example below, we apply an anonymous function in which each image in the collection is referred to by the 'img' variable name.

{% highlight javascript %}
// Calculate NDVI and add as a new band
// use "map" to apply an anonymous function to each image
var cNDVI = cMasked.map(function(img){
  // add a NDVI band to each image
  return img.addBands(img.normalizedDifference(['B5','B4']).rename('NDVI'));
});
{% endhighlight %}

### Image Mosaics: Create an Image Composite from Collection
We now need to assemble the image collection to create one continuous image across Washington state. There are several mosaicking/compositing options available, from simple maximum value composites (`imageCollection.max()`) and straightforward mosaics with the most recent image on top (`imageCollection.mosaic()`). The [Compositing and Mosaicking page on the Developer's Guide](https://developers.google.com/earth-engine/ic_composite_mosaic) provides examples of these.

Here, we will use the `imageCollection.qualityMosaic()` function. By prioritizing the image to use based on one specific band, this method ensures that the values across all bands are taken from the same image. Each pixel gets assigned the values from the image with the highest value of the desired band.

We will use this to make a "greenest pixel composite" for Washington state based on the NDVI band we just calculated. The final composite image will retain all bands in the input (unless we were to specify otherwise). Each pixel in the composite image could potentially come from imagery acquired on different dates, but all bands within each pixel are from the same image. In general, this provides a snapshot of the landscape at the peak of the growing season, regardless of the phenological timing within the year.

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

Annual maximum NDVI across Washington:

<br>
<img src="../fig/03_waNDVI.png" border = "10">
<br><br>

We can also use this composite image to visualize a true color composite using the RGB bands as we did above:

{% highlight javascript %}
// Visualize true color composite
Map.addLayer(greenest, {bands: ['B4', 'B3', 'B2'], min: 0, max: 2000}, 'WA tcc', false);
{% endhighlight %}

<br>
<img src="../fig/03_waTCC.png" border = "10">
<br><br>

## Exporting Composite Images
Users can export the results of their image manipulations to their GEE Asset folder for downstream use within GEE or to their personal Google Drive or Google Cloud Storage accounts. Here, we export a single-band image of annual maximum NDVI for Washington state. Examples are provided for asset and Google Drive exports. More information can be found [here in the Developers Guide](https://developers.google.com/earth-engine/exporting).

In the JavaScript API, all exports are sent to the 'Tasks' tab in the upper right panel. To prevent users from inadvertently overwhelming the system with gratuitous, accidental tasks, you need to explicitly run individual exports from the 'Tasks' tab. YOu can change filenames and other parameters here if necessary, or hard code these into your script.

When exporting to Google Drive, GEE will find the named folder specified and does not need the full file path. If this folder does not yet exist, it will create it for you in your Drive.

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
  assetId: 'users/yourname/2016_WA_ndvi',    // can add subfolders if needed
  scale: 30,
  region: boundary.geometry().bounds(),
  pyramidingPolicy: {'.default':'mean'}, // use {'.default':'sample'} for discrete data
  maxPixels: 2000000000
});
{% endhighlight %}

Link to a static version of the full script used in this module:
(https://code.earthengine.google.com/37ca6df552bd8c19488273ff9d4f444b)
