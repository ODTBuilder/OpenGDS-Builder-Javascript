[![License: LGPL v3](https://img.shields.io/badge/License-LGPL%20v3-blue.svg)](https://www.gnu.org/licenses/lgpl-3.0)
[![Korean](https://img.shields.io/badge/language-Korean-blue.svg)](#korean)


<a name="korean"></a>
# OpenGDS-Builder-Javascript
OpenGDS/Builder에서 공간정보기술(주)가 직접 개발한 Javascript library들입니다.</br>

# Quick Start

### 1. Openlayers 3 선언하기
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
**더 자세한 내용들은 https://openlayers.org/ 를 참조해주세요.<br>
### 2. OpenGDS/Builder 선언하기
```
<head>
  <link rel="stylesheet" href="https://openlayers.org/en/v4.6.5/css/ol.css" type="text/css">
  <link rel="stylesheet" href="https://location-of-gb/css/gb.css" type="text/css">
  <style>
    .map {
           height: 400px;
           width: 100%;
         }
  </style>
  <script src="https://openlayers.org/en/v4.6.5/build/ol.js" type="text/javascript"></script>
  <script src="https://location-of-gb/gb.js" type="text/javascript"></script>
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

  var temp = new gb.header.EditingTool({
              targetElement : jQuery객체, // EditingTool 메뉴바를 생성할 Div의 jQuery객체
              map : map, // 위에 선언한 ol.Map
              featureRecord : new gb.edit.FeatureRecord({
			        wfstURL : "geoserver/geoserverWFSTransaction.ajax", // Geoserver Layer 변경사항 저장 요청
			        layerInfoURL : "geoserver/getGeoLayerInfoList.ajax" // Geoserver Layer 정보 요청
		              }),
	      locale : "en", // 언어 설정
              layerInfo : "geoserver/getGeoLayerInfoList.ajax", // Geoserver Layer 정보 요청
              imageTile : "geoserver/geoserverWMSLayerLoad.do", // WMS 요청
              wfsURL : "geoserver/geoserverWFSGetFeature.ajax", // Feature 객체 요청
	      isEditing : gb.module.isEditing // EditingTool 활성화시 다른 작업을 제한하는 모듈
  });
</script>
```
** EditingTool에 사용된 주소들은 openGDS/Builder의 Java Controller 주소입니다.
![editingtool](https://user-images.githubusercontent.com/16248351/41519448-220ca6de-7303-11e8-863a-ca364eaf5a82.PNG)

#사용 라이브러리</br>
jsTree 3.3.3, 3.3.7 (MIT License)</br>
ol-geocoder 3.2.0 (MIT License)</br>
