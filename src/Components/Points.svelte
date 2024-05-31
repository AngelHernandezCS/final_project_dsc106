<script>
  import { onMount } from 'svelte';
  import * as d3 from 'd3';

  export let value;

  let width = 500;
  let height = 400;

  let svg;
  let airbasesGroup;
  let data;
  let airbasesData;
  let airportData;
  let airportGroup

  onMount(async () => {
    svg = d3.select("#map")
      .attr("width", width)
      .attr("height", height);

    const geoData = await d3.json('./data/states.json');
    const projection = d3.geoAlbersUsa().fitSize([width, height], geoData);
    const path = d3.geoPath().projection(projection);

    svg.append("g")
      .selectAll("path")
      .data(geoData.features)
      .enter().append("path")
      .attr("d", path)
      .attr("fill", d3.interpolatePurples(0.7))
      .attr("stroke", "#fff");

    // Load the locations data
    data = await d3.csv('./data/locations.csv');

    // Load the airbases data
    airbasesData = await d3.csv('./data/airbases.csv');

    airportData = await d3.csv('./data/airports.csv')

    // Add the locations
    const locationsGroup = svg.append("g");
    locationsGroup.selectAll("circle")
      .data(data)
      .enter().append("circle")
      .attr("cx", d => projection([+d.longitude, +d.latitude])[0])
      .attr("cy", d => projection([+d.longitude, +d.latitude])[1])
      .attr("r", 0.5)
      .attr("fill", "yellow")
      .attr("opacity", 1);

    // Add the airbases with initial opacity 0
    airbasesGroup = svg.append("g");
    airbasesGroup.selectAll("circle")
      .data(airbasesData)
      .enter().append("circle")
      .attr("cx", d => projection([+d.longitude, +d.latitude])[0])
      .attr("cy", d => projection([+d.longitude, +d.latitude])[1])
      .attr("r", 3)
      .attr("fill", "red")
      .style("transition", "opacity 1s ease") // CSS transition for opacity change
      .attr("opacity", 0); // Start with opacity 0

    airportGroup = svg.append('g');
    airportGroup.selectAll("circle")
      .data(airportData)
      .enter().append("circle")
      .attr("cx", d => projection([+d.longitude, +d.latitude])[0])
      .attr("cy", d => projection([+d.longitude, +d.latitude])[1])
      .attr("r", 4)
      .attr("fill", "#66FF00")
      .style("transition", "opacity 1s ease") // CSS transition for opacity change
      .attr("opacity", 0); // Start with opacity 0
  });

  // Reactive statement to update airbase styles based on value
  $: if (airbasesGroup) {
    
    if (value === 1) {
      airbasesGroup.selectAll('circle')
        .attr('opacity', 1);
      airportGroup.selectAll('circle')
        .attr('opacity', 0); 
      document.getElementById('title').innerText = "Sightings and Military Bases";
    } else if (value === 0){
      airbasesGroup.selectAll("circle")
        .attr("opacity", 0);
      airportGroup.selectAll('circle')
        .attr('opacity', 0)
      document.getElementById('title').innerText = "Locations of Sightings";
    } else if (value == 2){
      airportGroup.selectAll("circle")
      .attr('opacity', 1)
    }
  }
</script>

<div class="container">
  <h1 id="title">Locations of Sightings</h1>
  <div id="map-container">
    <svg id="map"></svg>
  </div>
</div>

<style>
  #title {
    text-align: center;
    margin-bottom: 20px;
    color: white;
  }

  #map-container {
    width: 100%;
    max-width: 800px;
    height: auto;
    display: block;
    overflow: hidden;
  }

  #map {
    width: 100%;
    height: 100%;
  }
</style>
