---
layout: post
title: "A2DD: Summer in the City"
date: 2014-11-16 13:38:00
---

<link rel="stylesheet" href="http://cdn.leafletjs.com/leaflet-0.7.3/leaflet.css" />
<style>

    #map {
        height: 700px;
        width: 100%;
    }
    h1 {
        font-family: "Lato";
        font-size: 18px;
    }

</style>

<div id="map"></div>

<script src="http://d3js.org/d3.v3.min.js" charset="utf-8"></script>
<script src="http://cdn.leafletjs.com/leaflet-0.7.3/leaflet.js"></script>
<script src="/leaflet-heat.js"></script>
<script src="/geoparsed.js"></script>
<script>

    var locations = [];

    var carpools = new L.LayerGroup();

    var RedIcon = L.Icon.Default.extend({
        options: {
            iconUrl: "/images/marker-icon-red.png"
        }
    });
    var redIcon = new RedIcon();
    d3.csv("/carpool-sites.csv", function(data) {
        data.forEach(function(d) {
            L.marker([d.Latitude, d.Longitude], {icon: redIcon}).bindPopup(d["Carpool Site"]).addTo(carpools);
        });
    });
        
    var map = L.map("map", {layers: [carpools]}).setView([42.39, -83.278], 10);
    var heat = L.heatLayer(addressPoints, {radius:45, blur:10}).addTo(map);
    var tiles = L.tileLayer('http://{s}.tile.osm.org/{z}/{x}/{y}.png', {
    }).addTo(map);

</script>

