<script>
    import { onMount } from 'svelte';
    import * as d3 from 'd3';
    
    let width = 800;
    let height = 600;
  
    onMount(async () => {
        const svg = d3.select("#base")
            .attr("width", width)
            .attr("height", height);
    
        const data = await d3.csv('./data/airbases.csv');
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
    
        svg.append("g")
            .selectAll("circle")
            .data(data)
            .enter().append("circle")
            .attr("cx", d => projection([+d.longitude, +d.latitude])[0])
            .attr("cy", d => projection([+d.longitude, +d.latitude])[1])
            .attr("r", 5)  // Adjust radius as needed
            .attr("fill", "red")
            .attr("opacity", 4);  // Fully visible points
    });
  </script>
  
  <div class="container">
    <h1 id="title">Air Force Bases and Sightings</h1>
    <div id="base-container">
        <svg id="base"></svg>
    </div>
  </div>
  
  <style>
    #title {
        text-align: center;
        margin-bottom: 20px;
        color: white;
    }
  
    #base-container {
        width: 100%;
        max-width: 800px;
        height: auto;
        display: block;
        overflow: hidden;
    }
  
    #base {
        width: 100%;
        height: 100%;
    }
  </style>
  