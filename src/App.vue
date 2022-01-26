<template>
  <div id="app">
    <h1>TomTom Maps Demo</h1>
    <div id="map" ref="mapRef"></div>
  </div>
</template>

<script>
import { onMounted, ref, reactive, watch } from 'vue';
import * as turf from "@turf/turf";
import axios from 'axios';


export default {
  name: 'App',
  setup() {
    const state = reactive({
      map: null,
    });
    const mapRef = ref(null);
    onMounted(() => {
      const tt = window.tt;
      var map = tt.map({
        key: 'ZKw6t4sUUNbnBPDkdRFnz3e3SAkGCpVp',
        container: mapRef.value,
        style: 'tomtom://vector/1/basic-main',
        zoom: 5,
        center: [12.3, 32.992578],
      });
      console.log("ddsf")
      map.addControl(new tt.FullscreenControl());
      map.addControl(new tt.NavigationControl());
      state.map = map;
      addMarker(map)
      console.log(state.map);
      map.on('click', function (event) {
        const position = event.lngLat;
        console.log(position);
        axios
          .post(
            'https://api.tomtom.com/geofencing/1/projects/1c2f4c1d-c1eb-43b3-a023-18e503469104/fence?key=ZKw6t4sUUNbnBPDkdRFnz3e3SAkGCpVp&adminKey=EZxCm4RTQdtBk3sIxqPgLIIG1Rkf87lZWllxckyob74wexys',
            {
              name: `Our location ${Math.floor(Math.random() * 10000)}`,
              type: 'Feature',
              geometry: {
                radius: 85,
                type: 'Point',
                shapeType: 'Circle',
                coordinates: [position.lat, position.lng],
              },
            }
          )
          .then((res) => {
            console.log(res.data);
            console.log(position)
            let _center = turf.point([position.lng,position.lat]);
            let _radius = 45;
            let _options = {
              steps: 80,
              units: 'kilometers' // or "mile"
            };

            let _circle = turf.circle(_center, _radius, _options);

            map.addSource("circleData", {
              type: "geojson",
              data: _circle,
            });

            map.addLayer({
              id: "circle-fill",
              type: "fill",
              source: "circleData",
              paint: {
                "fill-color": "red",
                "fill-opacity": 0.5,
              },
            });
            //map.setCenter([parseFloat(position.lat), parseFloat(position.lng)]);
          })
          .catch((err) => console.log(err));
      });
    });


    function addMarker(map) { 
    const tt = window.tt; 
    var location = [11.60798131506698, 31.67346189482023]; 
    var popupOffset = 25; 
 
    var marker = new tt.Marker().setLngLat(location).addTo(map); 
    var popup = new tt.Popup({ offset: popupOffset }).setHTML("Your address!"); 
            marker.setPopup(popup).togglePopup(); 
} 
    return {
      mapRef,
      state,
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
