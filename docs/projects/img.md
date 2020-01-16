# Images in Earth Engine

In the GEE environment images are stored in Cloud Optimized Geospatial tiles instead of a single image which allows for running an analysis this scale. This means that though the input imagery comes in know formats such as geotiff , MrSid and img these datasets post ingestion into GEE are converted into tiles that are used for at scale analysis. All images that are ingested into either GEE(s) Raster Catalog or your own personal folder and stored in folder or collections of images as you would expect to see when doing deep time stack analysis.

<center>![Imagery](../images/ee_imagery.gif)</center>

These images have defined data type,scales and projections along with some default properties such as an index and ID among other system properties. So we can query these properties, print them and add them

``` js
//Setting up a Geometry
var geometry = ee.FeatureCollection('users/io-work-1/vector/openca')

//PlanetScope is 4 band imagery, we are adding surface reflectance image with visualization
var vis = {"opacity":1,"bands":["b4","b3","b2"],"min":208,"max":2385,"gamma":1};

//Add an image
var image=ee.Image("users/io-work-1/open-ca/PS4BSR/20180621_182201_1008_3B_AnalyticMS_SR")
print("Single Image",image)

//Center the Map to the image and add the image
Map.centerObject(geometry,12)
Map.addLayer(image,vis,"Image")

//Clip an image
var clipped=image.clip(geometry)
Map.addLayer(clipped,vis,"Clipped Image")

//Change BaseMap to Satellite
Map.setOptions('SATELLITE')
```
