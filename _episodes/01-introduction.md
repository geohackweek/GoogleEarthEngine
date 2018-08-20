---
title: "Introduction to Google Earth Engine"
teaching: 5
exercises: 0
questions:
- "What is Google Earth Engine?"
- "What are the strengths and limitations of this platform?"
objectives:
- Understand the power of Google Earth Engine
- Basic understanding of the system architecture and philosophy
keypoints:
- " Google Earth Engine is a powerful tool for analyzing remote sensing imagery."
- " GEE's data catalog hosts a formidable variety of earth observation data."
- " GEE is changing how we understand and study the Earth."

---


## What is Google Earth Engine?

As Gorelick et al. (2017) write in the new *[Remote Sensing of the Environment](http://www.sciencedirect.com/science/article/pii/S0034425717302900)* article:

> Google Earth Engine is a cloud-based platform for planetary-scale
> geospatial analysis that brings Google's massive computational
> capabilities to bear on a variety of high-impact societal issues
> including deforestation, drought, disaster, disease, food security,
> water management, climate monitoring and environmental protection.

## Why use Google Earth Engine?

> Planetary Scale Change

Google Earth Engine enables users to compute on petabytes of data on the fly without having the navigate the complexities of cloud-based parallelization. Enhancing inclusive access has spurred the growth of environmental projects conducted at scales previously unimaginable.

Google Earth Engine brings together three key data science concepts:
 - Cloud archive of standard remote sensing + supporting data in one place (the cloud)
 - APIs to take your processing functions TO the data

### Reason # 1: Planetary-scale science

Since GEE came online, several ground-breaking studies have emerged that
demonstrate the power of bringing large-scale computing to bear on environmental and social problems. The first major use case, known as Global Forest Watch, fundamentally changed our understanding of forest loss on a global scale.


> Celebrity Use Case 1: The Hansen Data

 Example uses cases that have leveraged the full power of GEE include:

- [Global Forest Watch](http://www.globalforestwatch.org/) Global Forest Watch is a forest monitoring and conservation tool based on the the Hansen dataset. The Hansen dataset was created in Earth Engine and leverages the entire Landsat archive to map forest loss and gain at a global scale. [Hansen Tutorial for GEE](https://developers.google.com/earth-engine/tutorial_forest_01)

***

<img src="https://3c1703fe8d.site.internapcdn.net/newman/gfx/news/hires/2013/76fuygfd.gif" width="250" height="200" />


Forest loss in Sumatra's Riau province, Indonesia, 2000-2012. Credit: Hansen, Potapov, Moore, Hancher et al., 2013
***

- [Global Surface Water Occurrence Map](https://global-surface-water.appspot.com/) maps the temporal and spatial distribution of water on a global scale over the past three decades. [Global Surface Water Tutorial for GEE](https://developers.google.com/earth-engine/tutorial_global_surface_water_01)

- [Urban Travel Times](https://global-surface-water.appspot.com/) maps the temporal and spatial distribution of water on a global scale over the past three decades. [Global Surface Water Tutorial for GEE](https://developers.google.com/earth-engine/tutorial_global_surface_water_01)

### Reason # 2: Free cloud processing with built-in functions

Google Earth Engine is designed for cloud-based, parallelized geodata analysis. GEE takes care of all the parallelization on the back end for you. Those operations are called "server-side".

Using GEE, you can call a wide set of functions that have been developed specifically for computing in Earth Engine and apply them over many images simultaneously using Google computational infrastructure. No more downloading and analyzing individuals tiles at a time or stressing about your local storage.

### Reason # 3: Massive online public data archive

The GEE catalog hosts multiple petabytes of satellite imagery in the cloud, including the entire Landsat mission (!!). Other remote sensing missions represented include Sentinel 1, Sentinel 2, MODIS and others. In addition to earth observing images, GEE also hosts produced datasets for precipitation, population density, topography, land cover and climate. Over 6000 scenes are added daily from active satellite missions.
- [Table 1](http://www.sciencedirect.com/science/article/pii/S0034425717302900) in Gorelick et al. (2017) describes the frequently used datasets,
- the [Google Earth Engine](https://earthengine.google.com/datasets/) website has a general dataset descriptions,
- or you can browse the datasets directly through the [Google Earth Engine API](https://explorer.earthengine.google.com/#index).

<br>
<img src="../fig/01_datasets.png" border = "10" width="50%" height="50%">
<br><br>

### Reason # 4: Upload your own data

You can upload your own vector and raster data to the platform. You can also recommend datasets from the Javascript API Code Editor window by going the *Help* button on the top right and selecting *Suggest a dataset*.

<br>
<img src="../fig/01_datasetsuggest.png" border = "10">
<br><br>

### Reason # 5: Let them take care of version control

GEE will back up on your code in a git repository without you having to think about it. You can share those repositories with other users and view older versions of scripts easily from the Code Editor.


### Reason # 6: Flexible access through APIS

The GEE development team has worked hard to make GEE easy to access for a variety of different user types. Google Earth Engine can be accessed through a few different channels, including a non-programming GUI, the JavaScript API and the Python API.

For this class we are using the JavaScript API, but [training materials on accessing GEE using Python](https://developers.google.com/earth-engine/python_install) are now available on the GEE website.

<br>
<img src="../fig/01_GEEinterfaces.png" border = "10">
<br><br>

## How does it work?






On the scale of one particular research problem, other use cases might include:

- extracting and exporting a digital elevation model from a global dataset
- calculating a regional snow index from satellite imagery
- identifying [burn scars](https://gist.github.com/robinkraft/077c14d35a50a8b31581) or accessing near real time fire data  [FIRMS](https://earthdata.nasa.gov/earth-observation-data/near-real-time/firms)
- calculating a [time series](https://developers.google.com/earth-engine/charts_image_series_by_region) of precipitation over a study area
- creating a Global Flood Database like the startup [Cloud to Street](http://www.cloudtostreet.info/contact-us.html)



## Wrap Up

### What are common uses of GEE?

- operate on petabytes of imagery using Google's cloud
- embed outputs in apps
- store, share and version control your code
- import and export your own raster and vector data (assets)
- share your own raster and vector data
- export your analysis


### What is GEE NOT designed for?

- cartography
- heavy duty vector operations
- DIY parallelization. As stated in Gorelick et al. 2017, "The price of liberation from these details [of parallelization] is that the user is unable to influence them.”
