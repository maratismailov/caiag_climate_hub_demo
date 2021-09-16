<script>
  import L from "leaflet";
  import axios from "redaxios";
  import "../../public/leaflet/Bing.js";
  import "../../public/leaflet/Leaflet.Editable.js";
  import { onMount } from "svelte";
  // import "leaflet/dist/leaflet.css";
  let height = (window.innerHeight * 0.89).toString() + "px";
  let map;
  const lat = 7.816587
  const lon = 98.795328
  const center = [lat, lon];
  let zoom = 17;
  let id;
  let bing;
  let markerGroup;
  const popup = 'Название объекта: Bamboo Island' + '<br>Координаты объекта:' + lat + ' ' + lon 
  const api_key =
    "AijiWK2E56tAWqQiXj0TpzHR4V0xb0wDyCUzeUIqjbuPuwoFPP2kiWNi6TUVMpBn";

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
            console.log('dum')
          });
  });


  const showPosition = position => {
    lat = position.coords.latitude;
    lon = position.coords.longitude;
    center = [lat, lon];
  };

  const createMap = () => {
    map = L.map("map", {
      editable: true
    }).setView(center, zoom);
    bing = new L.BingLayer(api_key);
    map.addLayer(bing);
    markerGroup = L.layerGroup().addTo(map);
  };

  const resize = () => {
    height = (window.innerHeight * 0.89).toString() + "px";
  };
</script>

<style>
  .map {
    width: 95vw;
    margin: auto;
  }
</style>

<svelte:window on:resize={resize} on:orientationchange={resize} />

<div style="height: {height}" class="map" id="map">
  <slot />
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
