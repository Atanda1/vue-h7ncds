<template>
  <div id="app">
    <h1>Vue 3 TomTom Maps Demo</h1>
    <div id="map" ref="mapRef"></div>
  </div>
</template>

<script>
import { onMounted, ref, watch } from 'vue';

export default {
  name: 'App',
  setup() {
    const mapRef = ref(null);
    onMounted(() => {
      const tt = window.tt;
      var map = tt.map({
        key: 'ZKw6t4sUUNbnBPDkdRFnz3e3SAkGCpVp',
        container: mapRef.value,
        style: 'tomtom://vector/1/basic-main',
      });
      map.addControl(new tt.FullscreenControl());
      map.addControl(new tt.NavigationControl());
      map.on('click', function (event) {
        const position = event.lngLat;
        console.log(position);
       
      });
    });

    // function getCoordinates() {
    //   mapRef.value.on('click', function (event) {
    //     const position = event.lngLat;
    //     //   tt.services
    //     //     .reverseGeocode({
    //     //       key: apiKey,
    //     //       position: position,
    //     //     })
    //     //     .then(function (results) {
    //     //       drawPassengerMarkerOnMap(results);
    //     //     });
    //     // });

    //     //const position = event.lngLat;
    //     console.log(position);
    //   });
    // }

    return {
      mapRef
     // getCoordinates,
    };
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
#map {
  height: 60vh;
}
</style>