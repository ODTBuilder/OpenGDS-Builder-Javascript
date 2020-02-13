[![Korean](https://img.shields.io/badge/language-Korean-blue.svg)](#korean)


<a name="korean"></a>
# OpenGDS-Builder-Javascript
OpenGDS-Builder-Javascript는 OpenGDSBuilder2018Prod(https://github.com/ODTBuilder/OpenGDSBuilder2018Prod.git), OpenGDSBuilder2019Prod(https://github.com/ODTBuilder/OpenGDSBuilder2019Prod.git) 웹애플리케이션을 위해 공간정보기술(주)가 개발한 Javascript library들입니다. Layer 정보, Feature 속성, 그래픽 등의 공간 데이터에 대한 다양한 편집 기능을 제공하며 사용자 친화적인 UI로 공간정보를 보다 쉽게 다룰 수 있게 해줍니다.</br>
이 라이브러리들을 다운로드하여 기능별로 사용할 수 있으며 확장이 가능합니다.

# 특징
- 이 라이브러리들은 jQuery, Openlayers3, JSTree, Proj4js등을 사용하여 개발되었습니다.<br>
- 원하는 기능만을 선언하여 사용할 수 있어 편리합니다.<br>
- 공간 데이터를 웹브라우저상에서 쉽고 빠르게 관리할 수 있습니다.<br>
- 오픈소스 기반으로 개발되어 확장성이 뛰어납니다.<br><br>
일부 라이브러리들(검수, Geoserver 연동, 버저닝 등)은 작동을 위해 서버와의 통신이 필수적입니다.
서버와 연동되는 부분의 기능들은 OpenGDSBuilder2018Prod(https://github.com/ODTBuilder/OpenGDSBuilder2018Prod.git), OpenGDSBuilder2019Prod(https://github.com/ODTBuilder/OpenGDSBuilder2019Prod.git) 에서 확인할 수 있습니다.

# Quick Start

### 1. Openlayers Map 생성하기
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
  <%-- jsTree openlayers3--%>
  <script type="text/javascript" src="./jsTree-openlayers3/jstree.js"></script>
  <link rel="stylesheet" type="text/css"
    href="./jsTree-openlayers3/themes/default/style.css" />
  <script type="text/javascript"
    src="./jsTree-openlayers3/jstree-visibility.js"></script>
  <script type="text/javascript"
    src="./jsTree-openlayers3/jstree-layerproperties.js"></script>
  <script type="text/javascript"
    src="./jsTree-openlayers3/jstree-legends.js"></script>
  <script type="text/javascript"
    src="./jsTree-openlayers3/jstree-functionmarker.js"></script>
  <!-- gb.tree.openlayers -->
  <script src="./gb/tree/openlayers.js"></script>
  <!-- gb.edit -->
  <script src="./gb/edit/edithistory.js"></script>
  <script src="./gb/edit/undo.js"></script>
  <!-- gb.header -->
  <script src="./gb/header/base.js"></script>
  <script src="./gb/header/editingtool.js"></script>
  <title>OpenGDS/Builder example</title>
</head>
```
```
<body>
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
    "map" : gbMap.getUpperMap()
  });
  
  var temp = new gb.header.EditingTool({
    targetElement : gbMap.getLowerDiv(), // EditingTool 메뉴바를 생성할 Div의 jQuery객체
    map : gbMap.getUpperMap(), // ol.Map 객체
    otree : otree,
    featureRecord : new gb.edit.FeatureRecord(), // feature의 변경사항을 저장하는 객체
    locale : "en", // 언어 설정
    isEditing : gb.module.isEditing // EditingTool 활성화시 다른 작업을 제한하는 모듈
  });
</script>
</body>
```
<img src="https://user-images.githubusercontent.com/11713603/50584143-11137980-0eb1-11e9-8dc9-8ca533d129f9.png" alt="alt text" width="75%">

### 3. Openlayers Layer 객체 추적 기능 추가하기
```
<head>
  /**
   * 2번 header부분과 동일하게 작성
   */
   <script src="./gb/layer/navigator.js"></script>
</head>
```
```
<body>
 /* 2번 body부분과 동일하게 작성 */
</body>
```
<img src="https://user-images.githubusercontent.com/11713603/50585150-f9d78a80-0eb6-11e9-9fbb-065536614d7d.gif" alt="alt text" width="75%">

### 4. Editing Tool에 기능 추가하기
```
<head>
  /**
   * 2번 header부분과 동일하게 작성
   */
  <!-- gb.interaction -->
  <script src="./gb/interaction/measuretip.js"></script>
  <script src="./gb/interaction/holedraw.js"></script>
</head>
```
```
<body>
  <script>
    /* 2번 body부분과 동일하게 작성 */
    
    // 면적 측정 객체 선언
    var measureArea = new gb.interaction.MeasureTip({
      type : "Polygon",
      map : gbMap.getUpperMap(),
      snapSource : epan.snapSource
    });

    // 거리 측정 객체 선언
    var measureLength = new gb.interaction.MeasureTip({
      type : "LineString",
      map : gbMap.getUpperMap(),
      snapSource : epan.snapSource
    });
    
    // 홀 폴리곤 그리기 객체 선언
    var hole = new gb.interaction.HoleDraw({
      selected : epan.selected
    });
    
    // Editing Tool에 면적 측정 기능 추가
    epan.addInteraction({
      icon : "fas fa-ruler-combined",
      content : "area",
      interaction : measureArea,
      "float" : "right"
    });

    // Editing Tool에 거리 측정 기능 추가
    epan.addInteraction({
      icon : "fas fa-ruler-vertical",
      content : "length",
      interaction : measureLength,
      "float" : "right",
      clickEvent : function() {
        console.log("mesure length");
      }
    });
    
    // Editing Tool에 홀 폴리곤 그리기 기능 추가
    epan.addInteraction({
      icon : "fab fa-bitbucket",
      content : "Hole",
      interaction : hole,
      selectActive : true,
      "float" : "right",
      clickEvent : function() {
        console.log("Hole draw");
      }
    });
  <script>
</body>
** 위와 같은 방식을 통해 Editing Tool 메뉴에 새로운 기능을 추가할 수 있습니다.</br>
```
### 5. 3차원으로 확장하기
```
// CesiumJS 임포트
<script src="/resources/js/cesium/Cesium.js"></script>
<link rel="stylesheet" type="text/css" href="/resources/js/cesium/Widgets/widgets.css" />
// ThreeJS 임포트
<script src="${pageContext.request.contextPath}/resources/js/three/three.js"></script>
```

```
// 3차원 지도 객체 선언
var gb3dMap = new gb3d.Map({
			"gbMap" : gbMap, // 1번 과정에서 선언한 gbMap 변수를 생성자 파라미터로 입력
			"target" : $(".area-3d")[0], // 3차원 지도 영역 DIV 엘리먼트
			"initPosition" : [ 127.03250885009764, 37.51989305019379 ] // 최초 시작 지점
	});
    
** 자세한 선언 방법은 포함된 API를 참조바랍니다.    
```
![3차원 지도 화면](https://github.com/ODTBuilder/OpenGDS-Builder-Javascript/blob/master/screenshot/3dcapture.JPG) 

** icon 이미지는 fontawesome(https://fontawesome.com/icons)을 사용하였습니다.</br>
### 5. 사용 라이브러리
jQuery 2.2.2 (MIT License)</br>
DataTables 1.10.13 (MIT License)</br>
AltEditor 2.0 (MIT License)</br>
Fontawesosme 5.0.9 (MIT License)</br>
Bootstrap 3.3.2 (MIT License)</br>
spectrum 1.8.0 (MIT License)</br>
shp2geojson (MIT License)</br>
proj4js 2.2.1 (MIT License)</br>
jsTree 3.3.3, 3.3.7 (MIT License)</br>
ol-geocoder 3.2.0 (MIT License)</br>
Openlayers3 v5.3.0 (FreeBSD) www.openlayers.org</br>
ThreeJS r112 (MIT License)</br>
CesiumJS v 1.65 (Apache License 2.0)</br>
