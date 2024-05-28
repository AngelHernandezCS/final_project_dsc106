<script>
    import * as d3 from 'd3';
    import { onMount } from 'svelte';
    import * as topojson from 'topojson-client';
  
    let width = 800;
    let height = 600;
  
    onMount(async () => {
    const svg = d3.select("#map")
      .attr("width", width)
      .attr("height", height);

    // Load data and the GeoJSON file
    const data = await d3.csv('./data/locations.csv');
    const geoData = await d3.json('./data/states.json');
    

    // Create a projection and path generator
    const projection = d3.geoAlbersUsa().fitSize([width, height], geoData);
    const path = d3.geoPath().projection(projection);
  
      // Draw the map
      svg.append("g")
        .selectAll("path")
        .data(geoData.features)
        .enter().append("path")
        .attr("d", path)
        .attr("fill", d3.interpolatePurples(0.7))
        .attr("stroke", "#fff");
  
      // Add the points
      svg.append("g")
        .selectAll("circle")
        .data(data)
        .enter().append("circle")
        .attr("cx", d => projection([+d.longitude, +d.latitude])[0])
        .attr("cy", d => projection([+d.longitude, +d.latitude])[1])
        .attr("r", 1)
        .attr("fill", "yellow")
        .attr("opacity", 0.6)
        .append("title")
        .text(d => `(${d.latitude}, ${d.longitude})`);
    });
  </script>
  
  <div class="container">
    <h1 id="title">Locations of Sightings</h1>
    <svg id="map"></svg>
  </div>
  
  <style>
    #title {
    text-align: center;
    margin-bottom: 20px;
    color: white; /* Set title color to white for better contrast */
  }
  
    #map {
      width: 100%;
      max-width: 800px;
      height: auto;
      display: block;
    }
  </style>
  