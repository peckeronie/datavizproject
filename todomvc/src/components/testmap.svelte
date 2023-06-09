<script>
  import mapboxgl from "mapbox-gl";
  import { onMount } from "svelte";
  import mapData from "../data/map.json";
  export let geoJsonToFit;

  mapboxgl.accessToken =
    "pk.eyJ1IjoicGVpcWljIiwiYSI6ImNsZ2JseW13ZDA0ejUzcG85ZmJhN3RndXUifQ.Tbnw2SnV2zigv50y8sZerQ";

  let container;
  let map;

  let zoomLevel;

  function updateZoomLevel() {
    const screenWidth = window.innerWidth;
    zoomLevel = screenWidth <= 600 ? 4 : 5.85; // Adjust these values as needed
  }

  function handleResize() {
    updateZoomLevel();
    map.setZoom(zoomLevel);
  }

  onMount(() => {
    updateZoomLevel();
    map = new mapboxgl.Map({
      container,
      style: "mapbox://styles/mapbox/light-v11",
      center: [-87.6024, 12.0],
      zoom: 5.7,
      attributionControl: true, // removes attribution from the bottom of the map
    });

    window.addEventListener("resize", handleResize);

    map.addControl(new mapboxgl.FullscreenControl(),'bottom-left');

    map.on("load", () => {
      map.addLayer(
        {
          id: "country-boundaries",
          source: {
            type: "vector",
            url: "mapbox://mapbox.country-boundaries-v1",
          },
          "source-layer": "country_boundaries",
          type: "fill",
          paint: {
            "fill-color": "#EB5D11",
            "fill-opacity": 0.4,
          },
        },
        "country-label"
      );

      map.setFilter("country-boundaries", [
        "in",
        "iso_3166_1_alpha_3",
        "HND",
      ]);

      map.addLayer(
        {
          id: "country-boundaries-2",
          source: {
            type: "vector",
            url: "mapbox://mapbox.country-boundaries-v1",
          },
          "source-layer": "country_boundaries",
          type: "fill",
          paint: {
            "fill-color": "#107486",
            "fill-opacity": 0.4,
          },
        },
        "country-label"
      );

      map.setFilter("country-boundaries-2", [
        "in",
        "iso_3166_1_alpha_3",
        "GTM",
      ]);

      map.addLayer(
        {
          id: "country-boundaries-3",
          source: {
            type: "vector",
            url: "mapbox://mapbox.country-boundaries-v1",
          },
          "source-layer": "country_boundaries",
          type: "fill",
          paint: {
            "fill-color": "#F39600",
            "fill-opacity": 0.4,
          },
        },
        "country-label"
      );

      map.setFilter("country-boundaries-3", [
        "in",
        "iso_3166_1_alpha_3",
        "SLV",
      ]);

      // create a dictionary of colors
      const colors = {
        Honduras: "#CA543D",
        Guatemala: "#CA543D",
        "El Salvador": "#CA543D",
      };
      for (let i = 0; i < mapData.length; i++) {
        map.addSource(`source-${i}`, {
          type: "geojson",
          data: {
            type: "Feature",
            properties: {},
            geometry: {
              type: "LineString",
              coordinates: [
                [mapData[i].long_from, mapData[i].lat_from],
                [mapData[i].long_to, mapData[i].lat_to],
              ],
            },
          },
        });
        map.addLayer({
          id: `layer-${i}`,
          type: "line",
          source: `source-${i}`,
          paint: {
            "line-width": Math.pow(Math.log(mapData[i].popluation) / 2, 2),
            "line-color": colors[mapData[i].from],
            "line-opacity": 0.4,
          },
        });
      }

      updateBounds();
      map.on("zoom", updateBounds);
      map.on("drag", updateBounds);
      map.on("move", updateBounds);
    });
  });

  function updateBounds() {
    const bounds = map.getBounds();
    geoJsonToFit.features[0].geometry.coordinates = [
      bounds._ne.lng,
      bounds._ne.lat,
    ];
    geoJsonToFit.features[1].geometry.coordinates = [
      bounds._sw.lng,
      bounds._sw.lat,
    ];
  }
</script>

<svelte:head>
  <link
    rel="stylesheet"
    href="https://api.mapbox.com/mapbox-gl-js/v2.14.0/mapbox-gl.css"
  />
</svelte:head>

<div class="map" bind:this={container} />

<h2 class="title">The Amount of Emigrants from Central America</h2>

<a class="learn_more" href="/timeline">How do natural hazards influence this pattern? →</a>

<style>
  .map {
    width: 100vw;
    height: 96vw;
    position: fixed;
    transition: opacity 2s, visibility 2s;
    margin-left: 0px;
    margin-top:-70px;
    z-index: -1;
  }

  .title {
    position: absolute;
    bottom: 160px;
    left: 100px;
    color: #444;
    font-family: 'Open Sans', sans-serif;
  }

  .learn_more {
    font-size: 20px;
    position: absolute;
    bottom: 140px;
    left: 100px;
    text-decoration: none;
    color: #444;
  }

  .learn_more:hover {
    text-decoration: underline;
  }

  .learn_more:visited {
    color: #444;
  }
</style>
