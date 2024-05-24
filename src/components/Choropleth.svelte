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
    const data = await d3.csv('src/data/filtered_scrubbed.csv');
    const geoData = await d3.json('src/data/states.json');

    // Process data
    const dataMap = new Map(data.map(d => [d.state, +d.count]));
    // Create a color scale
    const colorScale = d3.scaleSequential(d3.interpolateBlues)
      .domain(d3.extent(data, d => +d.count));

    // Create a projection and path generator
    const projection = d3.geoAlbersUsa().fitSize([width, height], geoData);
    const path = d3.geoPath().projection(projection);

    // Draw the map
    svg.selectAll("path")
      .data(geoData.features)
      .enter().append("path")
      .attr("d", path)
      .attr("fill", d => {
        const stateName = d.properties.NAME;
        const value = dataMap.get(stateName);
        
        return value ? colorScale(value) : '#ccc';
      })
      .attr("stroke", "#333")
      .append("title")
      .text(d => {
        const stateName = d.properties.name;
        const value = dataMap.get(stateName);
        console.log(`${stateName}: ${value}`)
        return `${stateName}: ${value}`;
      });
  });
</script>

<svg id="choropleth"></svg>

<style>
  svg {
    width: 100%;
    height: auto;
  }
</style>
