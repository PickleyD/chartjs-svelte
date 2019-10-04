<script>
  import dispenseValuesData from "./data/dispensevalues.json";
  import dispenseValuesDrillsData from "./data/dispensevalues-drills.json";
  import BarChart from "./charts/BarChart.svelte";
  import DrillDownWrapper from "./chartsmeta/DrillDownWrapper.svelte";
</script>

<style>
  .chart-wrapper {
    width: 400px;
    height: 400px;
  }
</style>

<div class="chart-wrapper">
  <DrillDownWrapper>
    <!--Have to add a wrapper div for now as can't apply slot name to component. https://github.com/sveltejs/svelte/issues/1037 -->
    <div slot="top-level">
      <BarChart
        x={'date'}
        y={'dispenseValue'}
        drillable={true}
        data={dispenseValuesData}
        dataLabel={'Dispense value'} />
    </div>
    <div slot="bottom-level" let:selectedIndex={selIndex}>
      {#if selIndex != null}
        <BarChart
          x={'date'}
          y={'dispenseValue'}
          dataSelectedIndex={selIndex}
          data={dispenseValuesDrillsData}
          dataLabel={'Dispense value'} />
      {:else}
        <div>Select a bar from the chart above for a more detailed view.</div>
      {/if}
    </div>
  </DrillDownWrapper>
</div>
