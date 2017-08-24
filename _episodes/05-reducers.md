---
title: "Temporal and Spatial Reducers"
teaching: 5
exercises: 10
questions:
- "How do I summarize data for specific regions or time periods?"
objectives:
- Reduce an daily image collection to annual values
- Summarize values within polygon regions
- Use climate data products available through GEE
keypoints:
- " "
---

## Reducers: Overview

In Google Earth Engine (GEE), [reducers](https://developers.google.com/earth-engine/reducers_intro) are used to aggregate data over time, space, and other data structures. They belong to the `ee.Reducer` class and include summary statistics, histograms, and linear regression, among others. Here's a diagram from Google demonstrating a reducer applied to an image collection:

<img src="../fig/GEE_Reduce_ImageCollection.png" border = "10">

## Exercise 5: Obtain climate data
Here, we will demonstrate a temporal reducer and a spatial reducer by obtaining data on annual precipitation by US county. 

### GEE Data Catalog
A secondary objective to this exercise is to use GEE to access common datasets stored in the massive data archive that may appeal to those not directly interested in remote sensing applications. As described in _01 Introduction_, GEE has co-located a number of datasets relevant to earth systems analyses. The full archive can be browsed [here](https://code.earthengine.google.com/datasets/). In this exercise, we will use the [GRIDMET Meteorological Dataset](https://code.earthengine.google.com/dataset/IDAHO_EPSCOR/GRIDMET)  to obtain precipitation. Briefly, GRIDMET blends PRISM and NLDAS to produce a daily, 4 km gridded climate dataset for the contiguous United States from 1979 - present.

### 5.1: Reduce an ImageCollection to Aggregate Over Time
As discussed in Episode 03, an `ImageCollection` is a stack or time series of images. Reducers are used to derive a single `Image` based on the `ImageCollection`. Operations occur on a per pixel basis.

Processing Overview

* Load the GRIDMET data as an `ImageCollection` 
* Filter for the precipitation data band and dates desired (2016)
* _Reduce_ 365 "raster" images of daily precipitation into one raster image of annual precipitation totals (aka sum raster by pixel)

```javascript
// Temporal aggregations: a sum reducer ------------------------------------- 

// set year of interest
var year = 2016;

// get start date and end dates for year desired
var startDate = year + '-01-01';
var endDate = year + '-12-31';
  
// load precip data (mm, daily total): 365 images per year 
var cPrecip = ee.ImageCollection('IDAHO_EPSCOR/GRIDMET')
                    .select('pr')
                    .filterDate(startDate, endDate);
//print(cPrecip);  

// reduce the image collection to one image by summing the 365 daily rasters
var annualPrecip = cPrecip.sum();
//print(annualPrecip)

// visualize annual precipitation 
var precipPal = ['white','blue'] // store palette as variable
Map.addLayer(annualPrecip, {min: 60, max: 5900, palette: precipPal}, 'precip');
```