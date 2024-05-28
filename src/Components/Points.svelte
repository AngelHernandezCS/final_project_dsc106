<script>
    import { onMount } from 'svelte';
    import * as d3 from 'd3';
  
    let width = 800;
    let height = 600;
    let batchSize = 100; // Number of points to plot simultaneously
    let delay = 5; // Delay between each batch of points
  
    let pointsAnimated = false;
  
    onMount(async () => {
      const svg = d3.select("#map")
        .attr("width", width)
        .attr("height", height);
  
      const data = await d3.csv('./data/locations.csv');
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
  
      const pointsGroup = svg.append("g")
        .attr("class", "points-group");
  
      const circles = pointsGroup.selectAll("circle")
        .data(data)
        .enter().append("circle")
        .attr("cx", d => projection([+d.longitude, +d.latitude])[0])
        .attr("cy", d => projection([+d.longitude, +d.latitude])[1])
        .attr("r", 1)
        .attr("fill", "yellow")
        .attr("opacity", 0);
  
      // Initialize intersection observer
      const observer = new IntersectionObserver((entries, observer) => {
        entries.forEach(entry => {
          if (entry.isIntersecting && !pointsAnimated) {
            animatePoints(circles);
            pointsAnimated = true;
          }
        });
      });
  
      // Observe the map container
      observer.observe(document.querySelector('#map-container'));
  
      function animatePoints(selection) {
        let i = 0;
        let circlesArray = selection.nodes();
  
        function plotBatch() {
          let end = Math.min(i + batchSize, circlesArray.length);
          for (let j = i; j < end; j++) {
            circlesArray[j].setAttribute("opacity", 0.6);
          }
          i += batchSize;
          if (i < circlesArray.length) {
            setTimeout(plotBatch, delay);
          }
        }
  
        plotBatch();
      }
    });
  </script>
  
  <div class="container">
    <h1 id="title">Locations of Sightings</h1>
    <div id="map-container">
      <svg id="map"></svg>
    </div>
  </div>
  
  <style>

@keyframes popUp {
    from {
      opacity: 0;
      transform: translateX(-50px);
    }
    to {
      opacity: 1;
      transform: translateX(0);
    }
  }

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
      overflow: hidden; /* Ensure the SVG overflows from container for animation */
    }
  
    #map {
      width: 100%;
      height: 100%;
    }
  
    circle {
      fill: yellow;
      opacity: 0;
      animation: popUp 0.5s ease-in-out; /* Apply the pop-up animation */
    }
  </style>
  