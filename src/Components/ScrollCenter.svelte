  <script>
    import Scrolly from "./Scrolly.svelte";
    import Points from "./Points.svelte";
    let value;

    $: steps = [
      `<h1 class='step-title'>Step 1</h1>
      <br><br>
      <p>
        Lorem ipsum dolor sit amet consectetur adipisicing elit. Ipsum, exercitationem. Quam impedit fuga quia earum architecto qui temporibus deleniti obcaecati assumenda, minima consequatur a fugit?
      </p>`,
      `<h1 class='step-title'>Step 2</h1>
      <p>
        Lorem ipsum dolor sit amet consectetur adipisicing elit. Ipsum, exercitationem. Quam impedit fuga quia earum architecto qui temporibus deleniti obcaecati assumenda, minima consequatur a fugit?
      </p>`,
      `<h1 class='step-title'>Step 3</h1>
      <p>
        Lorem ipsum dolor sit amet consectetur adipisicing elit. Ipsum, exercitationem. Quam impedit fuga quia earum architecto qui temporibus deleniti obcaecati assumenda, minima consequatur a fugit?
      </p>`,
      `<h1 class='step-title'>Step 4</h1>
      <p>
        Lorem ipsum dolor sit amet consectetur adipisicing elit. Ipsum, exercitationem. Quam impedit fuga quia earum architecto qui temporibus deleniti obcaecati assumenda, minima consequatur a fugit?
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
      border: 5px solid var(--default);
    }

    .step.active .step-content {
      background: #f1f3f3ee;
      color: var(--squid-ink);
    }

    .spacer {
      height: 100vh;
    }
  </style>
