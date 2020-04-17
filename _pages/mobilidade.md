---
title: "Mobilidade"
permalink: /mobilidade/
layout: single
---


<html><link rel="stylesheet" href="https://unpkg.com/leaflet@1.6.0/dist/leaflet.css"
   integrity="sha512-xwE/Az9zrjBIphAcBb3F6JVqxf46+CDLwfLMHloNu6KEQCAWi6HcDUbeOfBIptF7tcCzusKFjFw2yuvEpDL9wQ=="
   crossorigin=""/>
	<title>Leaflet Layers Control Example</title>
	<script src="https://unpkg.com/leaflet@1.6.0/dist/leaflet.js"
   integrity="sha512-gZwIG9x3wUXg2hdXF6+rVkLF/0Vi9U8D2Ntg4Ga5I5BZpVkVxlJWbSQtXPSiUTtC0TjtGOmxa1AJPuV0CPthew=="
   crossorigin=""></script>


<style>	#mapid { width: 100%;
						height: 700px; 
			} 

	.info {
		 padding: 6px 8px;
		 font: 14px/16px Arial, Helvetica, sans-serif;
		 background: white;
		 background: rgba(255,255,255,0.8);
		 box-shadow: 0 0 15px rgba(0,0,0,0.2);
		 border-radius: 5px;
	}
	.info h4 {
		 margin: 0 0 5px;
		 color: #777;
	}

	.legend {
		 line-height: 18px;
		 color: #555;
	}
	.legend i {
		 width: 18px;
		 height: 18px;
		 float: left;
		 margin-right: 8px;
		 opacity: 0.7;
	}

</style>

<div id="mapid"></div>

	<script type="text/javascript" src="teste_mapa.js"></script>
	<script type="text/javascript">


	var geojson;

	var mymap = L.map('mapid').setView([39.961009, -8.143740], 6.5);
	var mapboxAccessToken = 'pk.eyJ1IjoiaHlnb3JwaWFnZXQiLCJhIjoiY2s4bHNyNzk0MDlhYzNvbzJ0d203NmZsdiJ9.RlmzkWL4eZ-eKiDBTCEQmQ';
	L.tileLayer('https://api.mapbox.com/styles/v1/{id}/tiles/{z}/{x}/{y}?access_token='+ mapboxAccessToken, {
		 attribution: 'Map data &copy; <a href="https://www.openstreetmap.org/">OpenStreetMap</a> contributors, <a href="https://creativecommons.org/licenses/by-sa/2.0/">CC-BY-SA</a>, Imagery © <a href="https://www.mapbox.com/">Mapbox</a>',
		 maxZoom: 18,
		 id: 'mapbox/light-v9',
		 tileSize: 512,
		 zoomOffset: -1,
		 accessToken: 'your.mapbox.access.token'
	}).addTo(mymap);


	function getColor(d) {
		 return d > 30 ? '#800026' :
		        d > 20  ? '#BD0026' :
		        d > 10  ? '#E31A1C' :
		        d > 0  ? '#FC4E2A' :
		        d > -10   ? '#FD8D3C' :
		        d > -20   ? '#FEB24C' :
		        d > -50   ? '#FED976' :
		                   '#FFEDA0';
	}

	function style(feature) {
		 return {
		     fillColor: getColor(feature.properties.Mobilidade),
		     weight: 1,
		     opacity: 0.2,
		     color: 'black',
		     dashArray: '3',
		     fillOpacity: 0.7
		 };
	}

	function highlightFeature(e) {
		 var layer = e.target;

		 layer.setStyle({
		     weight: 2,
		     color: 'black',
		     dashArray: '',
		     fillOpacity: 1.0
		 });
		 if (!L.Browser.ie && !L.Browser.opera && !L.Browser.edge) {
		     layer.bringToFront();
		 }
	    info.update(layer.feature.properties);

	}
	function resetHighlight(e) {
	    geojson.resetStyle(e.target);
	    info.update();

	}
	function zoomToFeature(e) {
		 map.fitBounds(e.target.getBounds());
	}

	function onEachFeature(feature, layer) {
		 layer.on({
		     mouseover: highlightFeature,
		     mouseout: resetHighlight,
		     click: zoomToFeature
		 });
	}


	var info = L.control();

	info.onAdd = function (mymap) {
		 this._div = L.DomUtil.create('div', 'info'); // create a div with a class "info"
		 this.update();
		 return this._div;
	};

	// method that we will use to update the control based on feature properties passed
	info.update = function (props) {
		 this._div.innerHTML = '<h4>Mobilidade Concelho</h4>' +  (props ?
		     '<b>' + props.NAME_2 + '</b><br />' + props.Mobilidade + '%'
		     : 'Passe o cursor sobre um concelho');
	};

	info.addTo(mymap);


	var legend = L.control({position: 'bottomright'});

	legend.onAdd = function (mymap) {

		 var div = L.DomUtil.create('div', 'info legend'),
		     grades = [-100, -50, -20, -10, 0., 10, 20, 30],
		     labels = [];

		 // loop through our density intervals and generate a label with a colored square for each interval
		 for (var i = 0; i < grades.length; i++) {
		     div.innerHTML +=
		         '<i style="background:' + getColor(grades[i] + 1) + '"></i> ' +
		         grades[i] + (grades[i + 1] ? ' &ndash; ' + grades[i + 1] + '<br>' : '+');
		 }

		 return div;
	};

	legend.addTo(mymap);

	geojson = L.geoJson(statesData, {
		 style: style,
		 onEachFeature: onEachFeature
	}).addTo(mymap);


	</script>
</html>
