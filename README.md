# OL3ClosesPoint
passo um para mapa de tubulação
<!doctype html>
<html lang="en">
  <head>
    <link rel="stylesheet" href="http://openlayers.org/en/v3.2.1/css/ol.css" type="text/css">
    <style>
      .map {
        height: 400px;
        width: 100%;
      }
    </style>
    <script src="http://openlayers.org/en/v3.2.1/build/ol.js" type="text/javascript"></script>
    <title>OpenLayers 3 example</title>
  </head>
  <body>
    <h2>My Map</h2>
    <div id="map" class="map"></div>
<script
	src="http://ajax.googleapis.com/ajax/libs/jquery/1.10.2/jquery.min.js"></script>
    <script type="text/javascript">
$.ajaxPrefilter(function(options) {
  if(options.crossDomain && jQuery.support.cors) {
    var http = (window.location.protocol === 'http:' ? 'http:' : 'https:');
    options.url = http + '//cors-anywhere.herokuapp.com/' + options.url;
    //options.url = "http://cors.corsproxy.io/url=" + options.url;
  }
});
var feature = new ol.Feature({
    'geometry': new ol.geom.Point(
        [-5494030.476636235, -1145898.5476357532])
  });
var vectorSource = new ol.source.Vector({
  features: [feature]
});
var vector = new ol.layer.Vector({
  source: vectorSource
});
var url = 'http://teste.hidroinformatica.org/geoserver254/topp/ows?service=WFS&version=1.0.0&request=GetFeature&typeName=topp:tasmania_roads&maxFeatures=50&outputFormat=application/json';
   var json = $.ajax({
			url : url,
			dataType : "json",
			async : false
		}).responseText;
		var obj = eval("(" + json + ")");
console.log(json.toString());
console.log(obj);
var format = new ol.format.GeoJSON();
            var vectorLayer = new ol.layer.Vector({
                source: new ol.source.StaticVector({
                    format: format,
                    projection: 'EPSG:3857'
                })
            });
var source = vectorLayer.getSource();
var json = JSON.parse(json.toString());
var features = source.readFeatures(json);
var source = vectorLayer.getSource();
source.addFeatures(features);
  var map = new ol.Map({
        target: 'map',
        layers: [
          new ol.layer.Tile({
            source: new ol.source.MapQuest({layer: 'sat'})
          }), vectorLayer, vector
        ],
        view: new ol.View({
          center: ol.proj.transform([37.41, 8.82], 'EPSG:4326', 'EPSG:3857'),
          zoom: 4
        })
      });

	function closestToRoad(){
console.log(source.getFeatures());
		var closestFeature = source.getClosestFeatureToCoordinate( [-5494030.476636235, -1145898.5476357532]);
 		var geometry = closestFeature.getGeometry();
		var closestPoint = geometry.getClosestPoint([-5494030.476636235, -1145898.5476357532]);
		console.log(closestPoint);
		var lineFeature = new ol.Feature(new ol.geom.LineString([[-5494030.476636235, -1145898.5476357532], closestPoint]));
		source.addFeature(lineFeature);
	}
closestToRoad();
    </script>
  </body>
</html>
