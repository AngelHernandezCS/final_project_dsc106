<script>
    import { onMount } from "svelte";
    import ChartJS from "chart.js/auto";

    export let dataset; // pass the entire dataset as a prop

    let ctx;
    let myChart;
    let selectedState = ""; // tracks the selected state

    $: filteredData = selectedState ? dataset.filter(item => item.state === selectedState) : [];

    $: if (myChart) {
        // update chart data when filtered data changes
        myChart.data.labels = filteredData.map(item => item.shape);
        myChart.data.datasets[0].data = filteredData.map(item => item.count);
        myChart.update();
    }

    onMount(async () => {
        myChart = new ChartJS(ctx, {
            type: "bar",
            data: {
                labels: [],
                datasets: [
                    {
                        label: "# of Sightings",
                        data: [],
                        backgroundColor: "rgba(54, 162, 235, 0.2)",
                        borderColor: "rgba(54, 162, 235, 1)",
                        borderWidth: 1,
                    },
                ],
            },
            options: {
                scales: {
                    y: {
                        beginAtZero: true,
                    },
                },
            },
        });
    });
</script>

<div class = 'menu'>
    <select bind:value={selectedState}>
        <option value="">Select a state</option>
        {#each [...new Set(dataset.map(item => item.state))] as state}
            <option value={state}>{state}</option>
        {/each}
    </select>
</div>

<div class="card">
    <canvas id="myChart" width="250" height="100" bind:this={ctx} />
</div>

<style>

    .card{
        background-color: 071630;
    }

    .menu{
        background-color: 071630;
    }
</style>