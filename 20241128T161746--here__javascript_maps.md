---
title:      "HERE"
date:       2024-11-28T16:17:46-04:00
tags:       ["javascript", "maps"]
identifier: "20241128T161746"
signature:  ""
---

Efficient map rendering - the API is built for WebGL and HTML5-capable environments 

the API provides full access to world-leading map data and map images,
with a choice of view modes and customization options, including three main map types: map, terrain and hybrid.

Geocoding - the API provides full access to geocoding and reverse geocoding services.

Routing - the API supports route calculation and display, 

Custom map objects - the API supports the creation of both interactive and non-interactive map objects: markers with SVG, HTML or bitmap images geo shapes, including polygons, polylines, circles and rectangles.

Mouse and touch interaction - The API (via an events extension) supports mouse and touch interaction with the map, including pan, zoom and pinch-to-zoom

Pre-built UI controls - The API offers pre-built, customizable UI controls that allow users to change the base map, zoom in and out smoothly, and display the current map scale. 


# Get API Key

first you need to obtain an API key from the HERE platform.

The HERE Maps API for JavaScript exposes a number of services.
These services need to be linked to your project for the API to work with those services. 

The first step for any application based on the HERE Maps API for JavaScript is to load the necessary code libraries or modules. 

The implementation of our basic use case requires two modules: *core* and *service* 
 
To load the modules, add the following <script> elements to the <head> of the HTML document:

``` html
<script src="https://js.api.here.com/v3/3.1/mapsjs-core.js"
  type="text/javascript" charset="utf-8"></script>
<script src="https://js.api.here.com/v3/3.1/mapsjs-service.js"
  type="text/javascript" charset="utf-8"></script>
```
 
To ensure optimum performance on mobile devices, add the following meta-tag to the <head> section of the HTML page:

``` html
<meta name="viewport" content="initial-scale=1.0, width=device-width" />
```

Here is the complete <head> element that loads the core and service modules and ensures optimum performance on mobile devices.

``` html
<!DOCTYPE html>
  <html>
    <head>
      ...
      <meta name="viewport" content="initial-scale=1.0,
        width=device-width" />
      <script src="https://js.api.here.com/v3/3.1/mapsjs-core.js"
        type="text/javascript" charset="utf-8"></script>
      <script src="https://js.api.here.com/v3/3.1/mapsjs-service.js"
        type="text/javascript" charset="utf-8"></script>
      ...
    </head>
    <body>
      <div style="width: 640px; height: 480px" id="mapContainer"></div>
```

An essential part of creating a working application with the HERE Maps API for JavaScript is to establish communication with the back-end services provided by HERE REST APIs. 

For this purpose, initialize a Platform object with the API key you received on registration:
``` javascript
var platform = new H.service.Platform({
  'apikey': '{YOUR_API_KEY}'
});

```

next

1. Create an HTML container element in which the map can be rendered (for example, a div).
2. Instantiate an H.Map object, specifying:
the map container element
the map type to use
the zoom level at which to display the map
the geographic coordinates of the point on which to center the map

``` javascript
// Obtain the default map types from the platform object:
var defaultLayers = platform.createDefaultLayers();

// Instantiate (and display) a map object:
var map = new H.Map(
    document.getElementById('mapContainer'),
    defaultLayers.vector.normal.map,
    {
      zoom: 10,
      center: { lat: 52.5, lng: 13.4 }
    });
```


complete example

``` html
<!DOCTYPE html>
<html>
  <head>
    <meta name="viewport" content="initial-scale=1.0, width=device-width" />
    <script src="https://js.api.here.com/v3/3.1/mapsjs-core.js"
    type="text/javascript" charset="utf-8"></script>
    <script src="https://js.api.here.com/v3/3.1/mapsjs-service.js"
    type="text/javascript" charset="utf-8"></script>
  </head>
  <body>
    <div style="width: 640px; height: 480px" id="mapContainer"></div>
    <script>
      // Initialize the platform object
      var platform = new H.service.Platform({
        'apikey': 'YOUR_API_KEY'
      });

      // Obtain the default map types from the platform object
      var maptypes = platform.createDefaultLayers();

      // Instantiate (and display) the map
      var map = new H.Map(
        document.getElementById('mapContainer'),
        maptypes.vector.normal.map,
        {
          zoom: 10,
          center: { lng: 13.4, lat: 52.51 }
        });
    </script>
  </body>
</html>
```
