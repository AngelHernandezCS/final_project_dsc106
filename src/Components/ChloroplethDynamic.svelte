<script>
    import * as d3 from 'd3';
    import { onMount } from 'svelte';
  
    let width = 800;
    let height = 600;
    let selectedHour = "00";
  
    const hours = Array.from({ length: 24 }, (_, i) => i.toString().padStart(2, '0'));
  
    let svg;
    let colorScale;
    let dataMap;
    let tooltip;
  
    // Function to load data based on selected hour
    async function loadData(hour) {
      const filename = `./data/sightings_${hour}.csv`;
      const data = await d3.csv(filename);
      const geoData = await d3.json('./data/states.json');
  
      // Process data
      dataMap = new Map(data.map(d => [d.state, +d.count]));
  
      // Update color scale based on data
      colorScale = d3.scaleSequential()
        .domain([d3.min(data, d => +d.count), d3.max(data, d => +d.count)])
        .interpolator(d3.interpolatePurples)
        .range([d3.interpolatePurples(0.3), d3.interpolatePurples(0.9)]);
  
      // Create a projection and path generator
      const projection = d3.geoAlbersUsa().fitSize([width, height], geoData);
      const path = d3.geoPath().projection(projection);
  
      // Update or draw the map
      svg.selectAll("path")
        .data(geoData.features)
        .join("path")
        .attr("d", path)
        .attr("fill", d => {
          const stateName = d.properties.NAME;
          const value = dataMap.get(stateName);
          return value ? colorScale(value) : d3.interpolatePurples(0.3);
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
  
      // Update legend
      updateLegend();
    }
  
    // Initialize the map on mount
    onMount(() => {
      svg = d3.select("#dynamic-choropleth")
        .attr("width", width)
        .attr("height", height);
  
      tooltip = d3.select("body").append("div")
        .attr("class", "tooltip")
        .style("position", "absolute")
        .style("background", "#fff")
        .style("padding", "5px")
        .style("border", "1px solid #ccc")
        .style("border-radius", "3px")
        .style("pointer-events", "none")
        .style("display", "none");
  
      loadData(selectedHour); // Load initial data
    });
  
    // Function to update legend
    function updateLegend() {
      const legend = d3.select("#dynamic-legend")
        .attr("width", 100)
        .attr("height", 200);
  
      const legendScale = d3.scaleLinear()
        .domain([d3.min(Array.from(dataMap.values())), d3.max(Array.from(dataMap.values()))])
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
        .tickPadding(5);
  
      legend.append("g")
        .attr("transform", "translate(30, 10)")
        .call(legendAxis)
        .style("fill", "white")
        .selectAll("text")
        .style("fill", "white");
    }
  
    // Watch for changes in selectedHour and update data accordingly
    $: {
      loadData(selectedHour);
    }
  </script>
  
  <div class="container">
    <h1 id="dynamic-chloro">Dynamic Number of UFO Sightings by State</h1>
  
    <div class="dropdown">
      <label for="hour-select">Select Hour: </label>
      <select id="hour-select" bind:value={selectedHour}>
        {#each hours as hour}
          <option value={hour}>{hour}:00</option>
        {/each}
      </select>
    </div>
  
    <svg id="dynamic-choropleth"></svg>
    <svg id="dynamic-legend"></svg>
  </div>
  
  <style>
    #dynamic-chloro {
      text-align: center;
      margin-bottom: 20px;
      color: white;
    }
  
    .dropdown {
      margin-bottom: 20px;
      text-align: center;
      color: white;
    }
  
    #dynamic-choropleth {
      width: 100%;
      max-width: 800px;
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
  
    #dynamic-legend {
      position: absolute;
      left: 18%;
      top: 152%;
      fill: white;
    }
  </style>
  