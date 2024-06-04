<script>
  import Meta from "./Meta.svelte";
  import Title from "./Components/Title.svelte";
  import Intro from "./Components/Intro.svelte";
  import ScrollCenter from "./Components/ScrollCenter.svelte";
  import Conclusion from "./Components/Conclusion.svelte";
  import Resources from "./Components/Resources.svelte";
  import Choropleth from "./Components/Choropleth.svelte";
  import Chorotext from "./Components/Chorotext.svelte";
  import Chart from "./Components/Chart.svelte";
  import Charttext from "./Components/Charttext.svelte";
  const dataset = [
    { state: "All States", shape: "light", count: 13471 },
    { state: "All States", shape: "triangle", count: 6548 },
    { state: "All States", shape: "circle", count: 6117 },
    { state: "Alabama", shape: "light", count: 129 },
    { state: "Alabama", shape: "triangle", count: 78 },
    { state: "Alabama", shape: "circle", count: 61 },
    { state: "Alaska", shape: "light", count: 89 },
    { state: "Alaska", shape: "circle", count: 29 },
    { state: "Alaska", shape: "disk", count: 27 },
    { state: "Arizona", shape: "light", count: 548 },
    { state: "Arizona", shape: "triangle", count: 236 },
    { state: "Arizona", shape: "circle", count: 210 },
    { state: "Arkansas", shape: "light", count: 122 },
    { state: "Arkansas", shape: "triangle", count: 85 },
    { state: "Arkansas", shape: "circle", count: 61 },
    { state: "California", shape: "light", count: 1829 },
    { state: "California", shape: "circle", count: 844 },
    { state: "California", shape: "triangle", count: 822 },
    { state: "Colorado", shape: "light", count: 293 },
    { state: "Colorado", shape: "triangle", count: 164 },
    { state: "Colorado", shape: "circle", count: 118 },
		{ state: "Connecticut", shape: "light", count: 160 },
    { state: "Connecticut", shape: "circle", count: 93 },
    { state: "Connecticut", shape: "fireball", count: 90 },
    { state: "Delaware", shape: "triangle", count: 30 },
    { state: "Delaware", shape: "light", count: 28 },
    { state: "Delaware", shape: "circle", count: 22 },
    { state: "Florida", shape: "light", count: 748 },
    { state: "Florida", shape: "fireball", count: 408 },
    { state: "Florida", shape: "triangle", count: 356 },
    { state: "Georgia", shape: "light", count: 246 },
    { state: "Georgia", shape: "circle", count: 144 },
    { state: "Georgia", shape: "triangle", count: 128 },
    { state: "Hawaii", shape: "light", count: 62 },
    { state: "Hawaii", shape: "circle", count: 32 },
    { state: "Hawaii", shape: "sphere", count: 29 },
    { state: "Idaho", shape: "light", count: 133 },
    { state: "Idaho", shape: "triangle", count: 49 },
    { state: "Idaho", shape: "circle", count: 45 },
    { state: "Illinois", shape: "light", count: 525 },
    { state: "Illinois", shape: "triangle", count: 299 },
    { state: "Illinois", shape: "circle", count: 270 },
    { state: "Indiana", shape: "light", count: 270 },
    { state: "Indiana", shape: "triangle", count: 142 },
    { state: "Indiana", shape: "fireball", count: 125 },
    { state: "Iowa", shape: "light", count: 168 },
    { state: "Iowa", shape: "triangle", count: 105 },
    { state: "Iowa", shape: "circle", count: 55 },
    { state: "Kansas", shape: "light", count: 138 },
    { state: "Kansas", shape: "triangle", count: 64 },
    { state: "Kansas", shape: "other", count: 50 },
    { state: "Kentucky", shape: "light", count: 167 },
    { state: "Kentucky", shape: "triangle", count: 97 },
    { state: "Kentucky", shape: "circle", count: 87 },
    { state: "Louisiana", shape: "light", count: 123 },
    { state: "Louisiana", shape: "triangle", count: 66 },
    { state: "Louisiana", shape: "circle", count: 58 },
    { state: "Maine", shape: "light", count: 127 },
    { state: "Maine", shape: "circle", count: 66 },
    { state: "Maine", shape: "triangle", count: 59 },
    { state: "Maryland", shape: "light", count: 160 },
    { state: "Maryland", shape: "triangle", count: 88 },
    { state: "Maryland", shape: "circle", count: 82 },
    { state: "Massachusetts", shape: "light", count: 238 },
    { state: "Massachusetts", shape: "triangle", count: 129 },
    { state: "Massachusetts", shape: "circle", count: 120 },
    { state: "Michigan", shape: "light", count: 373 },
    { state: "Michigan", shape: "triangle", count: 219 },
    { state: "Michigan", shape: "fireball", count: 171 },
    { state: "Minnesota", shape: "light", count: 215 },
    { state: "Minnesota", shape: "triangle", count: 124 },
    { state: "Minnesota", shape: "circle", count: 97 },
    { state: "Mississippi", shape: "light", count: 68 },
    { state: "Mississippi", shape: "triangle", count: 46 },
    { state: "Mississippi", shape: "circle", count: 40 },
    { state: "Missouri", shape: "light", count: 301 },
    { state: "Missouri", shape: "triangle", count: 178 },
    { state: "Missouri", shape: "fireball", count: 125 },
    { state: "Montana", shape: "light", count: 109 },
    { state: "Montana", shape: "fireball", count: 54 },
    { state: "Montana", shape: "unknown", count: 44 },
    { state: "Nebraska", shape: "light", count: 80 },
    { state: "Nebraska", shape: "circle", count: 35 },
    { state: "Nebraska", shape: "triangle", count: 34 },
    { state: "Nevada", shape: "light", count: 166 },
    { state: "Nevada", shape: "circle", count: 67 },
    { state: "Nevada", shape: "other", count: 67 },
    { state: "New Hampshire", shape: "light", count: 115 },
    { state: "New Hampshire", shape: "circle", count: 48 },
    { state: "New Hampshire", shape: "triangle", count: 43 },
    { state: "New Jersey", shape: "light", count: 240 },
    { state: "New Jersey", shape: "circle", count: 126 },
    { state: "New Jersey", shape: "disk", count: 115 },
    { state: "New Mexico", shape: "light", count: 148 },
    { state: "New Mexico", shape: "unknown", count: 66 },
    { state: "New Mexico", shape: "circle", count: 53 },
    { state: "New York", shape: "light", count: 572 },
    { state: "New York", shape: "circle", count: 307 },
    { state: "New York", shape: "triangle", count: 291 },
    { state: "North Carolina", shape: "light", count: 354 },
    { state: "North Carolina", shape: "circle", count: 182 },
    { state: "North Carolina", shape: "triangle", count: 174 },
    { state: "North Dakota", shape: "triangle", count: 23 },
    { state: "North Dakota", shape: "light", count: 20 },
    { state: "North Dakota", shape: "other", count: 13 },
    { state: "Ohio", shape: "light", count: 438 },
    { state: "Ohio", shape: "triangle", count: 252 },
    { state: "Ohio", shape: "circle", count: 225 },
    { state: "Oklahoma", shape: "light", count: 150 },
    { state: "Oklahoma", shape: "triangle", count: 69 },
    { state: "Oklahoma", shape: "circle", count: 67 },
    { state: "Oregon", shape: "light", count: 397 },
    { state: "Oregon", shape: "circle", count: 172 },
    { state: "Oregon", shape: "triangle", count: 150 },
    { state: "Pennsylvania", shape: "light", count: 468 },
    { state: "Pennsylvania", shape: "circle", count: 231 },
    { state: "Pennsylvania", shape: "triangle", count: 223 },
    { state: "Rhode Island", shape: "light", count: 41 },
    { state: "Rhode Island", shape: "circle", count: 30 },
    { state: "Rhode Island", shape: "triangle", count: 22 },
    { state: "South Carolina", shape: "light", count: 256 },
    { state: "South Carolina", shape: "circle", count: 111 },
    { state: "South Carolina", shape: "triangle", count: 84 },
    { state: "South Dakota", shape: "light", count: 47 },
    { state: "South Dakota", shape: "triangle", count: 20 },
    { state: "South Dakota", shape: "unknown", count: 19 },
    { state: "Tennessee", shape: "light", count: 198 },
    { state: "Tennessee", shape: "triangle", count: 120 },
    { state: "Tennessee", shape: "circle", count: 99 },
    { state: "Texas", shape: "light", count: 704 },
    { state: "Texas", shape: "triangle", count: 362 },
    { state: "Texas", shape: "circle", count: 285 },
    { state: "Utah", shape: "light", count: 131 },
    { state: "Utah", shape: "triangle", count: 66 },
    { state: "Utah", shape: "circle", count: 61 },
    { state: "Vermont", shape: "light", count: 59 },
    { state: "Vermont", shape: "fireball", count: 24 },
    { state: "Vermont", shape: "circle", count: 23 },
    { state: "Virginia", shape: "light", count: 229 },
    { state: "Virginia", shape: "triangle", count: 134 },
    { state: "Virginia", shape: "circle", count: 131 },
    { state: "Washington", shape: "light", count: 871 },
    { state: "Washington", shape: "fireball", count: 355 },
    { state: "Washington", shape: "triangle", count: 340 },
    { state: "West Virginia", shape: "light", count: 96 },
    { state: "West Virginia", shape: "triangle", count: 54 },
    { state: "West Virginia", shape: "circle", count: 45 },
    { state: "Wisconsin", shape: "light", count: 275 },
    { state: "Wisconsin", shape: "circle", count: 126 },
    { state: "Wisconsin", shape: "fireball", count: 120 },
    { state: "Wyoming", shape: "light", count: 47 },
    { state: "Wyoming", shape: "other", count: 17 },
    { state: "Wyoming", shape: "circle", count: 15 }
];
</script>
<div class = 'center'>
 <Meta />

</div>

<div class = 'center'>
  <Title />
</div>

<div class = 'center'>
  <Intro />
</div>
<div class="center">
  <Choropleth/>
</div>

<div class = 'center'>
  <Chorotext/>
</div>
<div class = 'center'>
  <ScrollCenter/>
</div>


<div class = 'bar'>
  <h1 class = 'bar' >UFO Sightings by Shape by State</h1>
  <!-- pass the hardcoded dataset as a prop to the chart component -->
  <Chart {dataset} />
</div>

<div class = 'center'>
<Charttext/>

</div>













<div class = 'center'>
  <Conclusion />
</div>

<Resources />

<style>

.center {
    display: flex;
    justify-content: center;
    align-items: center;
    width: 100%;
    background-color: #071630;
  }

.bar{
  background-color: #071630;
  color: white;
  text-align: center;
  margin: 0;
  padding: 0em 2em;
}
</style>