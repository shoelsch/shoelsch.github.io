---
layout: post
title: "Mapping Summer in the City Volunteers"
date: 2014-11-16 13:38:00
summary: "A mapping tool for deciding where carpool sites should be located to
accomodated a non-profit's volunteers"
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

Summer in the City is a non-profit organization based in Detroit that works to
bring a diverse group of volunteers together to address the immediate needs of
Detroit through their "Paint, Plant, and Play" program. Over the entire summer,
nearly 1,000 volunteers are driven from different carpool sites in the Metro Detroit
area to volunteer locations. The problem, though, is that because they offer
their volunteers 14 different carpool sites, on any given day, one site could be
overcapacity and another could be empty. And juggling transportation among 
these different sites can be laborious. 

Our solution was to reduce the number of carpool sites by aggregating nearby
carpool sites together to mitigate the problem of wildly varying carpool site
attendances. But then, how can Summer in the City decide which carpool sites to
(re)move?

I created this tool below to aid their decision making. The locations of approximately 2,000 volunteers
from the summers of 2013 and 2014 are mapped alongside the carpool locations. Denser areas
(signified by bright yellow, orange, and red) indicate the need for a nearby
carpool sites, but too many carpool sites nearby one another indicates the
opportunity for merging.

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

