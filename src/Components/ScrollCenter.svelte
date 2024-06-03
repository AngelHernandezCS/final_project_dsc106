  <script>
    import Scrolly from "./Scrolly.svelte";
    import Points from "./Points.svelte";
    let value;

    $: steps = [
      `<h1 class='step-title'>East Coast > West Coast ??</h1>
      <br><br>
      <p>
        Now that each sighting has been plotted in their exact locations we can take a better look at
        where to go to catch some UFOS. Although California has the most sightings it is fairly spread out 
        across the state. The largest cluster of sightings appears on the east coast. The entire northeast
        has more collective sightings in a smaller and denser area than California. Regardless, from the map
        you'll be able to pinpoint prime spots near you to and hopefully spot some UFOS.
      </p>
      <br>
      <br>
      <p>
        But hold on, if you're to truly take this seriously then we need to filter all these clusters
        and find the best location for authentic UFO sights. Let's not kid ourselves here some of these sightings
        have to be fake or misinterpreted. We want a chance at a real sighting! Of course one of the main conspiracies
        in UFO sightings is that they are top secret military technology. Lets see if this can be explained.
      </p>
      `,
      `<h1 class='step-title'>Area 51 is Empty?</h1>
      <p>
        Plotting known military Airforce bases helps us understand a few of the clusters on the map.
        Surprisingly Area 51, in Nevada, isnt littered with sightings given all the rumors that surround it.
        The two biggest clusters in Northern/Southern California surround an airforce base. Given many military planes
        can travel far and fast, many sightings can be explained by the military. A multitude of small clusters 
        in the central states follow the same pattern. Who knows, maybe the US Airforce is storing UFOS themselves?
      </p>`,
      `<h1 class='step-title'>Inter-Planetary Airports</h1>
      <p>
        Another explanation for some of these sightings is regular planes being mistaken for something more than it is.
        Given how big new Airbus Models are becoming and the really bright lights they emit during the night, it isn't
        a surprise people mistake them for something else. Now, in the NorthEast the majority of the clusters line up and
        surround heavy traffic airports. It would be best to avoid the NorthEast. So then what exactly should you be looking
        for? The best area has to be an area with alot of sightings and a good amount of distance from high traffic airports and
        Airforce bases.
      </p>`,
      `<h1 class='step-title'>Eureka!</h1>
      <p>
        The best spot is in the Mid-East. A little surprising since those states aren't any many tourists destination lists.
        But who cares when the aliens are there and there isnt a military base to accuse of hiding them. This zone has some airports but they are all spread out and many sightings occur
        inbetween the airports. The Mid-East is also a prime a location for clear night skies due to significantly less light
        pollution compared to the rest of the US. This zone if your best shot to catch a glimpse of a real UFO.
        Now we need to figure out what time you should be looking out for them.
      </p>`,

    ];


  </script>

  <section class="container">
    <!-- Fixed map container -->
    <div class="map-container">
      <Points {value} />
    </div>

    <!-- scroll container -->
    <div class="steps-container">
      <Scrolly bind:value>
        {#each steps as text, i}
          <div>
            <div class="step" class:active={value === i}>
              <div class="step-content">{@html text}</div>
            </div>
          </div>
        {/each}
        <div class="spacer" />
      </Scrolly>
    </div>
  </section>

  <style>
    .container {
      display: flex;
      width: 100%;
    }

    .map-container {
      flex: 0 0 40%;
      position: sticky;
      top: 20%;
      left: 10%;
      height: 100vh;
    }

    .steps-container {
      flex: 1;
      overflow-y: auto;
    }

    .step {
      height: 110vh;
      display: flex;
      align-items: center;
      justify-content: center;
    }

    .step-content {
      font-size: 18px;
      background: var(--bg);
      color: #ccc;
      border-radius: 1px;
      padding: 0.5rem 1rem;
      display: flex;
      flex-direction: column;
      justify-content: center;
      transition: background 500ms ease;
      text-align: left;
      width: 75%;
      margin: auto;
      max-width: 500px;
      font-family: var(--font-main);
      line-height: 1.3;
      border: 10px solid var(--default);
      border-color: purple;
    }

    .step.active .step-content {
      background: #f1f3f3ee;
      color: var(--squid-ink);
    }

    .spacer {
      height: 100vh;
    }
  </style>
