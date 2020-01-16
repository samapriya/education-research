# Edge Detection

You can also run powerful functions such as edge detection using Hough and Canny transforms for example for single images as well as on collections to do edge counts, connectivity measures among a few other applications.

<center>![Edges](../images/edge_detection.gif)</center>

Similar to earlier example you can access the [full script here](https://code.earthengine.google.com/e2bcba4015def1b97f297091abb7905e) or copy and past the same code into [code.earthengine.google.com](https://code.earthengine.google.com)

``` js
//Add an AOI
var aoi=ee.FeatureCollection("users/io-work-1/vector/subset")

//Zoom to AOI
Map.centerObject(aoi,15)

//Add image and visualization
var image=ee.Image("users/io-work-1/open-ca/PS4BSR/20180621_182201_1008_3B_AnalyticMS_SR")
var vis = {"opacity":1,"bands":["b4","b3","b2"],"min":-433.8386769876429,"max":2822.7077530529555,"gamma":1};

var ndvi = image.normalizedDifference(['b4', 'b3']);

// Apply a Canny edge detector.
var canny = ee.Algorithms.CannyEdgeDetector({
  image: ndvi,
  threshold: 0.2
}).multiply(255);

// Apply the Hough transform.
var h = ee.Algorithms.HoughTransform({
  image: canny,
  gridSize: 256,
  inputThreshold: 80,
  lineThreshold: 80
});

// Display.
Map.addLayer(image, vis, 'source_image');
Map.addLayer(ndvi,{min: -0.05, max: 0.5}, 'NDVI',false)
Map.addLayer(canny.updateMask(canny), {min: 0, max: 1, palette: 'blue'}, 'canny');
Map.addLayer(h.updateMask(h), {min: 0, max: 1, palette: 'red'}, 'hough');
Map.setOptions('SATELLITE')
```
