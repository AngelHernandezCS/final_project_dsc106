<script>
  import Meta from "./Meta.svelte";
  import Title from "./Components/Title.svelte";
  import Intro from "./Components/Intro.svelte";
  import TextAndMathEquations from "./Components/TextAndMathEquations.svelte";
  import LineChart from "./Components/LineChart.svelte";
  import ScrollCenter from "./Components/ScrollCenter.svelte";
  import Conclusion from "./Components/Conclusion.svelte";
  import Resources from "./Components/Resources.svelte";
  import ScrollSide from "./Components/ScrollSide.svelte";
  import Choropleth from "./Components/Choropleth.svelte";
  import Points from "./Components/Points.svelte";
</script>

<Meta />
<Title />
<Intro />


<div class="center">
  <Choropleth/>
</div>

<Points/>
<!--
after choropleth(s) should be a way to show user
the amount of sightings there are at a certain time (7am,7pm,12pm)
by itself using aggregate of all states, and an option to choose a specific state
for the given time (all states, CA, NV,..)

idea:
a center number and two drop down menus above it for each option
center number changes based on each drop down menu choice


-->

<ScrollSide />
<!--
use the scroll side template to show the most common shapes people have seen:
u can do ranking of each type or use the comments section in the dataset and find 
funny quotes people have said
idk but goal is to let people know what shapes to look for in the sky

-->
<Conclusion />
<Resources />

<style>

.center {
    display: flex;
    justify-content: center;
    align-items: center;
    width: 100%;
  }


</style>