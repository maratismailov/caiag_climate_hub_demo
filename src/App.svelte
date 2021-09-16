<script>
  import { Route, active } from "tinro";
  import Location from "./components/Location.svelte";
  import Photo from "./components/Photo.svelte";
  import L1_1 from "./components/L1_1.svelte";
  import L1_2 from "./components/L1_2.svelte";
</script>

<style>

</style>

<main>
  <div class="scrollmenu">
    <a href="/" use:active exact>L1_1</a>
    <!-- <a href="/l1_2" use:active exact>L1_2</a> -->
  </div>

</main>

<Route path="/photo">
  <Photo />
</Route>
<Route path="/location">
  <Location />
</Route>
<Route path="/">
  <L1_1 />
</Route>
<Route path="/l1_2">
  <L1_2 />
</Route>
<!-- <Route path="/currentdustazure">
  <CurrentDustAzure />
</Route>
<Route path="/heatmap">
  <Heatmap />
</Route> -->
