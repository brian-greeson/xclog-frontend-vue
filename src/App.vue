<template>
  <div id="app">
    <AppHeader></AppHeader>
    <div class="ui grid">
      <div class="row">
        <div class="twelve wide column">
          <FlightStats></FlightStats>
          <FlightMap></FlightMap>
        </div>
        <div class="four wide column">
          <FlightList></FlightList>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import AppHeader from "./components/AppHeader";
import FlightList from "./components/FlightList";
import FlightStats from "./components/FlightStats";
import FlightMap from "./components/FlightMap";

export default {
  name: "App",
  components: {
    AppHeader,
    FlightList,
    FlightStats,
    FlightMap,
  },
};
</script>

<style>
</style>
