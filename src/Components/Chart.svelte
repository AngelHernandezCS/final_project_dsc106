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
                        data: [],
                        backgroundColor: ["rgba(54, 162, 235, 0.2)", "rgba(0, 214, 76, 0.7)", "rgba(153, 102, 255, 0.2)"], // array of different colors
                        borderColor: ["rgba(54, 162, 235, 1)", "rgba(0, 214, 76, 0.7)", "rgba(153, 102, 255, 1)"], // corresponding border colors
                        borderWidth: 1,
                    },
                ],
            },
            options: {
                scales: {
                    x: {
                        ticks: {
                            color: 'white', // set the color of the x-axis labels to white
                            font: {
                                size: 17 // set the font size of the x-axis labels
                            }
                        },
                        title: {
                            display: true,
                            text: 'Description Type',
                            color: 'white', // Set the color of the y-axis title to white
                            font: {
                                size: 20 // Set the font size of the y-axis title
                            }
                        }
                    },
                    y: {
                        ticks: {
                            color: 'white' // sets the color of the y-axis labels to white
                        },
                        title: {
                            display: true,
                            text: 'Number of Sightings',
                            color: 'white', // Set the color of the y-axis title to white
                            font: {
                                size: 20 // Set the font size of the y-axis title
                            }
                        },
                        beginAtZero: true,
                    },
                },
                plugins: {
                    legend: {
                        labels: {
                            color: 'white' // sets the color of the legend labels to white
                        },
                        display: false // hide the legend item
                    }
                }
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
        background-color: #071630;
        width: 100%;
    }

    .menu{
        background-color: #071630;
        text-align: center;
        transform: translate(0px,18px);
    }
</style>