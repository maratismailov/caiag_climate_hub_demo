<script>
  import L from "leaflet";
  import axios from "redaxios";
  import "../../public/leaflet/Bing.js";
  import "../../public/leaflet/Leaflet.Editable.js";
  import { onMount } from "svelte";
  // import "leaflet/dist/leaflet.css";
  let height = (window.innerHeight * 0.89).toString() + "px";
  let map;
  const lat = 42.867972;
  const lon = 74.589243;
  const center = [lat, lon];
  let zoom = 8;
  let id;
  let bing;
  let wms_layer;
  let markerGroup;
  const popup = "Название объекта: Гимназия-комплекс №70" + "<br>Координаты объекта:" + lat + " " + lon;
  const api_key = "AijiWK2E56tAWqQiXj0TpzHR4V0xb0wDyCUzeUIqjbuPuwoFPP2kiWNi6TUVMpBn";

  onMount(() => {
    const getLocation = () => {
      if (navigator.geolocation) {
        navigator.geolocation.getCurrentPosition(showPosition);
      } else {
        location = "Geolocation is not supported by this browser.";
      }
    };
    createMap();
    L.marker([lat, lon])
      .bindPopup(popup)
      .addTo(markerGroup)
      .on("click", () => {
        console.log("dum");
      });
  });

  const showPosition = (position) => {
    lat = position.coords.latitude;
    lon = position.coords.longitude;
    center = [lat, lon];
  };

  let selected_layer = 0

  const layers = [
    { title: "Optical", type: "RGB", description: "rgb_comp", lowName: "ModisRGB", highName: "S2RGB", style: "rgb" },
    { title: "Pasture", type: "NDVI", description: "ndvi", lowName: "ModisIndices", highName: "MonthIndices", style: "ndvi" },
    { title: "Pasture", type: "Anomaly", description: "anomaly", lowName: "ModisAnomaly", highName: "MonthAnomaly", style: "anomaly" },
    { title: "Pasture", type: "Biomass", description: "biomass", lowName: "ModisPastureBiomass", highName: "MonthPastureBiomass", style: "pasture_biomass" },
    { title: "Pasture", type: "Trend", description: "trend", lowName: "ModisTrend", highName: "MonthTrend", style: "trend" },
    { title: "Snow", type: "SnowPercentage", description: "snow_percentage", lowName: "ModisSnowPercentage", highName: "MonthSnowPercentage", style: "snow_percentage" },
    { title: "Snow", type: "NDSI", description: "ndsi", lowName: "ModisIndices", highName: "MonthIndices", style: "ndsi" },
    { title: "Drought", type: "NDDI", description: "nddi", lowName: "ModisIndices", highName: "MonthIndices", style: "nddi" },
    { title: "Drought", type: "VCI", description: "vci", lowName: "ModisVCI", highName: "ModisVCI", style: "vci" },
    { title: "Drought", type: "VHI", description: "vhi", lowName: "ModisVHI", highName: "ModisVHI", style: "vhi" },
    { title: "Temperature", type: "LST", description: "lst", lowName: "ModisLST", highName: "ModisLST", style: "lst" },
  ];

  const wmsOptions = {
    layers: layers[selected_layer].lowName,
    format: "image/png",
  };

  const createMap = () => {
    map = L.map("map", {
      editable: true,
    }).setView(center, zoom);
    // bing = new L.BingLayer(api_key);
    // map.addLayer(bing);
    // L.tileLayer("https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png", {
    //   attribution: '&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors',
    // }).addTo(map);
    wms_layer = L.tileLayer.wms("https://kyrgyzstan.sibelius-datacube.org:5000/", wmsOptions).addTo(map);
    markerGroup = L.layerGroup().addTo(map);
  };

  const change_layer = selected_layer => {
    wms_layer.setParams({ layers: layers[selected_layer].lowName });
  }

  const resize = () => {
    height = (window.innerHeight * 0.89).toString() + "px";
  };
</script>

<svelte:window on:resize={resize} on:orientationchange={resize} />

<div>
  <div class="select">
    <select bind:value={selected_layer} on:change="{() => change_layer(selected_layer)}">
      {#each layers as layer, index}
        <option value={index}>{layer.title} - {layer.type}</option>
      {/each}
    </select>
  </div>

  <div style="height: {height}" class="map" id="map">
    <slot />
  </div>
</div>

<br />

<!-- <button class="action" on:click={refresh_area}>Refresh Selected</button>
<button class="action" on:click={save_changes}>Save changes</button>
<button class="action" on:click={show_map_object}>Show L.map</button>
<br />
<button class="action" on:click={start_polygon}>Start Polygon</button>
<button class="action" on:click={stop_drawing}>Stop drawing</button>
<button class="action" on:click={add_to_current}>Add to current</button>
<br />
<button class="action" on:click={show_url}>Show url</button>
<button class="action" on:click={add_to_cache}>Add to cache</button> -->

<!-- {#if area != ''}
  {#each area._layers[area_id - 1]._latlngs[0][0] as coordinate, index}
    <p>{index}</p>
    <p>{coordinate}</p>
  {/each}
{/if} -->
<style>
  .map {
    width: 95vw;
    margin: auto;
  }
  .select {
    width: 35vw;
    margin: auto;
    padding-bottom: 5px;
  }
</style>
