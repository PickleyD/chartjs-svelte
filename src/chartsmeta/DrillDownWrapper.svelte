<script context="module">
  export const drillDownCtx = {};
</script>

<script>
  import { setContext } from "svelte";
  import { writable } from "svelte/store";

  let selectedIndex = writable(null);

  setContext(drillDownCtx, {
    getIndex: () => selectedIndex,
    setIndex: i => {
      $selectedIndex = i;
    }
  });
</script>

<slot name="top-level">
  <div>No top level chart to display!</div>
</slot>

<slot name="bottom-level" selectedIndex={$selectedIndex}>
  <div>Selected index in parent chart is: {$selectedIndex}</div>
</slot>
