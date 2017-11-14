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


## Why GEE? Planetary Scale Change: The Hansen Dataset

Google Earth Engine enables users to compute on petabytes of data on the fly without having the navigate the complexities of cloud-based parallelization. Enhancing inclusive access has spurred the growth of environmental projects conducted at scales previously unimaginable. Example uses cases that have leveraged the full power of GEE include:

- [Global Forest Watch](http://www.globalforestwatch.org/) Global Forest Watch is a forest monitoring and conservation tool based on the the Hansen dataset. The Hansen dataset was created in Earth Engine and leverages the entire Landsat archive to map forest loss and gain at a global scale. [Hansen Tutorial for GEE](https://developers.google.com/earth-engine/tutorial_forest_01)

- [Global Surface Water Occurrence Map](https://global-surface-water.appspot.com/) maps the temporal and spatial distribution of water on a global scale over the past three decades. [Global Surface Water Tutorial for GEE](https://developers.google.com/earth-engine/tutorial_global_surface_water_01)

***

![alt text](https://3c1703fe8d.site.internapcdn.net/newman/gfx/news/hires/2013/76fuygfd.gif)

Forest loss in Sumatra's Riau province, Indonesia, 2000-2012. Credit: Hansen, Potapov, Moore, Hancher et al., 2013
***

Other use cases might include:

- extracting and exporting a digital elevation model from a global dataset
- calculating a regional snow index from satellite imagery
- identifying [burn scars](https://gist.github.com/robinkraft/077c14d35a50a8b31581) or accessing near real time fire data  [FIRMS](https://earthdata.nasa.gov/earth-observation-data/near-real-time/firms)
- calculating a [time series](https://developers.google.com/earth-engine/charts_image_series_by_region) of precipitation over a study area
- creating a Global Flood Database [Cloud to Street](http://www.cloudtostreet.info/contact-us.html)


## How does it work?

Google Earth Engine brings together three key data science concepts:
 - Cloud archive of standard remote sensing + supporting data in one place (the cloud)
 - Free massive cloud processing with algorithm primitives
 - Co-locating computing with data

You can access Google Earth Engine through a few different channels, including a non-programming GUI, the JavaScript API and the Python API. For this class we are using the JavaScript API, but [training materials on accessing GEE using Python](https://developers.google.com/earth-engine/python_install) are now available on the GEE website.

<br>
<img src="../fig/01_GEEinterfaces.png" border = "10">
<br><br>

### What datasets are available?

The GEE catalog hosts multiple petabytes of satellite imagery in the cloud, including the entire Landsat mission (!!). Other remote sensing missions represented include Sentinel 1, Sentinel 2, MODIS and others. In addition to earth observing images, GEE also hosts produced datasets for precipitation, population density, topography, land cover and climate. Over 6000 scenes are added daily from active satellite missions.
- [Table 1](http://www.sciencedirect.com/science/article/pii/S0034425717302900) in Gorelick et al. (2017) describes the frequently used datasets,
- the [Google Earth Engine](https://earthengine.google.com/datasets/) website has a general dataset descriptions,
- or you can browse the datasets directly through the [Google Earth Engine API](https://explorer.earthengine.google.com/#index).

<br>
<img src="../fig/01_datasets.png" border = "10">
<br><br>

### Can I suggest datasets?

Users can also recommend datasets from the Javascript API Code Editor window by going the *Help* button on the top right and selecting *Suggest a dataset*.

<br>
<img src="../fig/01_datasetsuggest.png" border = "10">
<br><br>


### What are common uses of GEE?

- operate on petabytes of imagery using Google's cloud
- export your analysis
- import and export your own raster and vector data (assets)
- share your own raster and vector data
- embed outputs in apps
- store, share and version control your code

### What is GEE NOT designed for?

- cartography
- heavy duty vector operations
- DIY parallelization. As stated in Gorelick et al. 2017, "The price of liberation from these details [of parallelization] is that the user is unable to influence them.”
