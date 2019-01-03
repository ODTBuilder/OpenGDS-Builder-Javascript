[![License: LGPL v3](https://img.shields.io/badge/License-LGPL%20v3-blue.svg)](https://www.gnu.org/licenses/lgpl-3.0)
[![Korean](https://img.shields.io/badge/language-Korean-blue.svg)](#korean)


<a name="korean"></a>
# OpenGDS-Builder-Javascript
OpenGDSBuilder2018Prod(https://github.com/ODTBuilder/OpenGDSBuilder2018Prod.git)에서 공간정보기술(주)가 직접 개발한 Javascript library들입니다.</br>
이 라이브러리들을 다운로드하여 기능별로 사용할 수 있으며 확장이 가능합니다.

# 특징
이 라이브러리들은 Openlayers3 와 JSTree를 사용하여 개발되었습니다. 일부 라이브러리들은 작동을 위해 서버와의 통신이 필수적입니다.

# Quick Start

### 1. Openlayers 3 선언하기
```
<!doctype html>
<html lang="en">
  <head>
    <script src="https://cdn.rawgit.com/openlayers/openlayers.github.io/master/en/v5.3.0/build/ol.js"></script>
    <link rel="stylesheet" href="https://cdn.rawgit.com/openlayers/openlayers.github.io/master/en/v5.3.0/css/ol.css" type="text/css">
    <link rel="stylesheet" href="./gb/css/gb.css">
    <script src="./gb/gb.js"></script>
    <script src="./gb/map/map.js"></script>
    <title>OpenGDS/Builder example</title>
  </head>
  <body>
    <h2>My Map</h2>
    <div id="map" class="map"></div>
    <script type="text/javascript">
      var gbMap = new gb.Map({
        "target" : $("#map")[0], // Openlayers Map을 생성할 HTML Element 객체
      });
    </script>
  </body>
</html>
```
**더 자세한 내용들은 https://openlayers.org/ 를 참조해주세요.<br>
### 2. Editing Tool 라이브러리 사용 환경 구성하기
```
<head>
  <script src="https://cdn.rawgit.com/openlayers/openlayers.github.io/master/en/v5.3.0/build/ol.js"></script>
  <link rel="stylesheet" href="https://cdn.rawgit.com/openlayers/openlayers.github.io/master/en/v5.3.0/css/ol.css" type="text/css">
  <link rel="stylesheet" href="./gb/css/gb.css">
  <script src="./gb/gb.js"></script>
  <script src="./gb/map/map.js"></script>
  <title>OpenGDS/Builder example</title>
</head>
```
```
<div class="builderLayer">
  <div class="builderLayerClientPanel"></div>
</div>
<div class="bind"></div>

<script type="text/javascript">
  var gbMap = new gb.Map({
    "target" : $(".bind")[0] // Openlayers Map을 생성할 HTML Element 객체
  });
  
  var otree = new gb.tree.OpenLayers({
    "append" : $(".builderLayerClientPanel")[0], // Openlayers Tree를 생성할 HTML Element 객체
    "map" : gbMap.getUpperMap(),
    "url" : {
        "getLegend" : "geoserver/geoserverWMSGetLegendGraphic.ajax" // 범례를 생성하기 위해 Geoserver로부터 데이터를 요청할 주소
    }
  });
  
  var temp = new gb.header.EditingTool({
    targetElement : gbMap.getLowerDiv(), // EditingTool 메뉴바를 생성할 Div의 jQuery객체
    map : gbMap.getUpperMap(), // ol.Map 객체
    otree : otree,
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

### 3. Geoserver 연동하기
```
<head>
  <link rel="stylesheet" href="https://openlayers.org/en/v4.6.5/css/ol.css" type="text/css">
  <link rel="stylesheet" href="https://location-of-gb/css/gb.css" type="text/css">
  <script src="https://openlayers.org/en/v4.6.5/build/ol.js" type="text/javascript"></script>
  <script src="https://location-of-gb/gb.js" type="text/javascript"></script>
  <title>OpenGDS/Builder example</title>
</head>
```
```
<div class="builderLayer">
  <div class="builderLayerGeoServerPanel"></div>
  <div class="builderLayerClientPanel"></div>
</div>
<div class="bind"></div>

<script type="text/javascript">
  var gbMap = new gb.Map({
    "target" : $(".bind")[0] // Openlayers Map을 생성할 HTML Element 객체
  });
  
  var otree = new gb.tree.OpenLayers({
    "append" : $(".builderLayerClientPanel")[0], // Openlayers Tree를 생성할 HTML Element 객체
    "map" : gbMap.getUpperMap(),
    "url" : {
        "getLegend" : "url.ajax" // 범례를 생성하기 위해 Geoserver로부터 데이터를 요청할 주소
    }
  });
  
  var gtree = new gb.tree.GeoServer({
    "locale" : "en",
    "append" : $(".builderLayerGeoServerPanel")[0], // Geoserver Tree를 생성할 HTML Element 객체
    "clientTree" : otree.getJSTree(),
    "map" : gbMap.getUpperMap(),
    "properties" : new gb.edit.ModifyLayerProperties({ // Geoserver Layer 정보 변경 기능 추가
        featureRecord : new gb.edit.FeatureRecord({
          wfstURL : "geoserver/geoserverWFSTransaction.ajax", // Geoserver Layer 변경사항 저장 요청
          layerInfoURL : "geoserver/getGeoLayerInfoList.ajax" // Geoserver Layer 정보 요청
        }),
    }),
    "uploadSHP" : new gb.geoserver.UploadSHP({ // Geoserver에 shp파일을 업로드하는 기능 추가
        "url" : "geoserver/upload.do" // Geoserver 파일 업로드 요청 주소
    }),
    "url" : {
        "getTree" : "geoserver/getGeolayerCollectionTree.ajax",
        "addGeoServer" : "geoserver/addGeoserver.ajax",
        "deleteGeoServer" : "geoserver/removeGeoserver.ajax",
        "deleteGeoServerLayer" : "geoserver/geoserverRemoveLayers.ajax",
        "getMapWMS" : "geoserver/geoserverWMSGetMap.ajax",
        "getLayerInfo" : "geoserver/getGeoLayerInfoList.ajax",
        "getWFSFeature" : "geoserver/geoserverWFSGetFeature.ajax",
        "switchGeoGigBranch" : "geoserver/updateGeogigGsStore.do",
    }
  });
</script>
```
** EditingTool에 사용된 주소들은 openGDS/Builder의 Java Controller 주소입니다.

#사용 라이브러리</br>
jsTree 3.3.3, 3.3.7 (MIT License)</br>
ol-geocoder 3.2.0 (MIT License)</br>
