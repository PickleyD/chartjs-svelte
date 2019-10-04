<script>
  import { onMount, getContext } from "svelte";
  import { drillDownCtx } from "../chartsmeta/DrillDownWrapper.svelte";
  import Chart from "chart.js";

  export let data;
  export let dataLabel;
  export let x;
  export let y;
  export let drillable = false;
  export let dataSelectedIndex;

  let canvas;
  let barChart;
  
  $: labels = dataSelectedIndex != null
    ? data[dataSelectedIndex].map(d => d[x])
    : data.map(d => d[x]);

  $: values = dataSelectedIndex != null
    ? data[dataSelectedIndex].map(d => d[y])
    : data.map(d => d[y]);

  $: {
    labels && values && drawChart();
  }

  const { getIndex, setIndex } = getContext(drillDownCtx);

  const handleClick = evt => {
    if (drillable && setIndex) {
      var activeElement = barChart.getElementAtEvent(evt);
      if (activeElement && activeElement[0]) {
        setIndex(activeElement[0]._index);
      }
    }
  };

  onMount(async () => {
    drawChart();
  });

  const drawChart = () => {
    if (canvas) {
      const ctx = canvas.getContext("2d");

      // clean up any event listeners and references from a previous chart
      if (barChart){
          barChart.destroy();
      }

      barChart = new Chart(ctx, {
        type: "bar",
        data: {
          labels,
          datasets: [
            {
              label: dataLabel,
              data: values,
              backgroundColor: "#0892d0",
              borderColor: "black"
            }
          ]
        },
        options: {
          scales: {
            yAxes: [
              {
                ticks: {
                  beginAtZero: true
                }
              }
            ]
          },
          onClick: handleClick
        }
      });
    }
  };
</script>

<canvas bind:this={canvas} width="100" height="100" />
