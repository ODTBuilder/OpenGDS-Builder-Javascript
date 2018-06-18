# OpenGDS-Builder-Javascript
OpenGDS/Builder 중 Javascript library</br>
License - LGPL 3.0</br>
jQuery, OpenLayers3, JsTree, jQueryUI, Bootstrap3를 필요로 합니다.</br>

# Quick Start

### 1. declare Openlayers 3
```
<!doctype html>
<html lang="en">
  <head>
    <link rel="stylesheet" href="https://openlayers.org/en/v4.6.5/css/ol.css" type="text/css">
    <style>
      .map {
        height: 400px;
        width: 100%;
      }
    </style>
    <script src="https://openlayers.org/en/v4.6.5/build/ol.js" type="text/javascript"></script>
    <title>OpenGDS/Builder example</title>
  </head>
  <body>
    <h2>My Map</h2>
    <div id="map" class="map"></div>
    <script type="text/javascript">
      var vector = new ol.layer.Vector({
        source: new ol.source.Vector({
          url: 'https://openlayers.org/en/v4.6.5/examples/data/geojson/countries.geojson',
          format: new ol.format.GeoJSON(),
          wrapX: false
        })
      });

      var map = new ol.Map({
        target: 'map',
        layers: [
          new ol.layer.Tile({
            source: new ol.source.OSM()
          }),
          vector
        ],
        view: new ol.View({
          center: ol.proj.fromLonLat([37.41, 8.82]),
          zoom: 4
        })
      });
    </script>
  </body>
</html>
```
### 2. declare OpenGDS/Builder
```
 <head>
    <link rel="stylesheet" href="https://openlayers.org/en/v4.6.5/css/ol.css" type="text/css">
    <link rel="stylesheet" href="https://location/of/gb/css/gb.css" type="text/css">
    <style>
      .map {
        height: 400px;
        width: 100%;
      }
    </style>
    <script src="https://openlayers.org/en/v4.6.5/build/ol.js" type="text/javascript"></script>
    <script src="https://location/of/gb/gb.js" type="text/javascript"></script>
    <title>OpenGDS/Builder example</title>
 </head>
```

```
<script type="text/javascript">
   var map = new ol.Map({
        target: 'map',
        layers: [
          new ol.layer.Tile({
            source: new ol.source.OSM()
          }),
          vector
        ],
        view: new ol.View({
          center: ol.proj.fromLonLat([37.41, 8.82]),
          zoom: 4
        })
      });

  var temp = new gb.panel.EditingTool({
              width : 84,
              height : 145,
              positionX : 425,
              positionY : 100,
              autoOpen : false,
              map : map, // 위에 선언한 ol.Map
              featureRecord : new gb.edit.FeatureRecord({
			        id : "feature_id" // GeoServer 통신시 feature의 고유ID로 사용되는 컬럼명
		              }),
              selected : function() { // 편집할 ol.layer.Base 객체를 반환할 함수
                return vector;
              },
              layerInfo : "http:// some geoserver url /geoserver/wms",
              imageTile : "http:// some geoserver url /geoserver/wms",
              getFeature : "http:// some geoserver url /geoserver/wfs",
              getFeatureInfo : "http:// some geoserver url /geoserver/wms"
  });
</script>
```
![editingtool](https://user-images.githubusercontent.com/16248351/41519448-220ca6de-7303-11e8-863a-ca364eaf5a82.PNG)
