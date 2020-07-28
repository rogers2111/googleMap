<template>
    <div id="App"></div>
</template>

<script>
import gmapsInit from './utils/gmaps';
import 'regenerator-runtime/runtime';
export default {
  name: 'App',

  data() {
      return {

      }
  },
  async mounted() {
      try {
          const google = await gmapsInit();
          new google.maps.Map(document.getElementById("App"), { zoom: 8, center: {lat: 31.717831, lng: -84.247732}});
      } catch ( error ) {
          console.log(error);
      }
  }  
}

</script>

<style>
html,
body {
  margin: 0;
  padding: 0;
}

#App {
  width: 100vw;
  height: 100vh;
}
</style>