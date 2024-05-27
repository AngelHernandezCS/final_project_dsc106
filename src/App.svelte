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
</script>

<Meta />
<Title />
<Intro />
<ScrollSide />

<div class="center">
  <Choropleth/>
</div>
<ScrollCenter />
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