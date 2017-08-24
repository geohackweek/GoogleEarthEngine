---
title: "Temporal and Spatial Reducers"
teaching: 15
exercises: 0
questions:
- "How do I summarize data for specific regions or time periods?"
objectives:
- Reduce an daily image collection to annual values
- Summarize values within polygon regions
- Use climate data products available through GEE
keypoints:
- " "
---

Test javaScript syntax highlighting

```js
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