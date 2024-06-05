<script>
  import * as d3 from 'd3';
  import { onMount } from 'svelte';

  let width = 800;
  let height = 600;

  onMount(async () => {
    const svg = d3.select("#choropleth")
      .attr("width", width)
      .attr("height", height);

    // Load data and the GeoJSON file
    const data = await d3.csv('./data/filtered_scrubbed.csv');
    const geoData = await d3.json('./data/states.json');

    // Process data
    const dataMap = new Map(data.map(d => [d.state, +d.count]));
    
    // Create a color scale
    const colorScale = d3.scaleSequential()
  .domain([d3.min(data, d => +d.count), d3.max(data, d => +d.count)])
  .interpolator(d3.interpolatePurples) // Using d3.interpolatePurples
  .range([d3.interpolatePurples(0.3), d3.interpolatePurples(0.9)]); 

    // Create a projection and path generator
    const projection = d3.geoAlbersUsa().fitSize([width, height], geoData);
    const path = d3.geoPath().projection(projection);

    const tooltip = d3.select("body").append("div")
      .attr("class", "tooltip")
      .style("position", "absolute")
      .style("background", "#fff")
      .style("padding", "5px")
      .style("border", "1px solid #ccc")
      .style("border-radius", "3px")
      .style("pointer-events", "none")
      .style("display", "none");

    // Draw the map
    svg.selectAll("path")
      .data(geoData.features)
      .enter().append("path")
      .attr("d", path)
      .attr("fill", d => {
        const stateName = d.properties.NAME;
        const value = dataMap.get(stateName);
        return value ? colorScale(value) : d3.interpolatePurples(0.3); // Ensure states with no data are set to a distinct color
      })
      .attr("stroke", "#fff")
      .attr("stroke-width", 0.5)
      .on("mouseover", function(event, d) {
        const stateName = d.properties.NAME;
        const value = dataMap.get(stateName);
        tooltip.style("display", "block")
          .html(`<strong>${stateName}</strong>: ${value || 0}`);
      })
      .on("mousemove", function(event) {
        tooltip.style("left", (event.pageX + 10) + "px")
          .style("top", (event.pageY - 20) + "px");
      })
      .on("mouseout", function() {
        tooltip.style("display", "none");
      });

      const legend = d3.select("#legend")
      .attr("width", 100)
      .attr("height", 200);

    const legendScale = d3.scaleLinear()
      .domain([d3.min(data, d => +d.count), d3.max(data, d => +d.count)])
      .range([0, 200]);

    const legendGradient = legend.append("defs")
      .append("linearGradient")
      .attr("id", "legendGradient")
      .attr("x1", "0%")
      .attr("x2", "0%")
      .attr("y1", "0%")
      .attr("y2", "100%");

    legendGradient.append("stop")
      .attr("offset", "0%")
      .attr("stop-color", d3.interpolatePurples(0.3));

    legendGradient.append("stop")
      .attr("offset", "100%")
      .attr("stop-color", d3.interpolatePurples(0.9));

    legend.append("rect")
      .attr("x", 10)
      .attr("y", 10)
      .attr("width", 20)
      .attr("height", 200)
      .attr("fill", "url(#legendGradient)");

      const legendAxis = d3.axisRight(legendScale)
  .tickSize(6)
  .ticks(6)
  .tickPadding(5); // Add some padding between ticks and text

legend.append("g")
  .attr("transform", "translate(30, 10)")
  .call(legendAxis)
  .style("fill", "white")
  .selectAll("text") // Select all tick text elements
  .style("fill", "white");

      
      

  });
</script>

<div class="container">
  <h1 id="chloro">Number   of UFO Sightings by State</h1>
  
  <svg id="choropleth"></svg>
  <svg id = 'legend'></svg>
  
</div>

<style>
  #chloro {
    text-align: center;
    margin-bottom: 20px;
    color: white; /* Set title color to white for better contrast */
  }

  #choropleth {
    width: 100%;
    max-width: 800px; /* Optional: restrict maximum width */
    height: auto;
    display: block;
  }


  .tooltip {
    background-color: white;
    color: black;
    border: 1px solid #ccc;
    padding: 5px;
    border-radius: 3px;
    pointer-events: none;
    position: absolute;
    display: none;
  }
  #legend {
    position: absolute;
    fill: white;
  }

  #body-text{
    color: white;
  }

 




</style>
