<template>
    <div class="App"></div>
</template>

<script>
import gmapsInit from '../utils/gmaps';
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
        const map = new google.maps.Map(this.$el);
            map.setCenter({lat: 51.513329, lng: -0.088950});

        } catch ( error ) {
        console.log(error);
    }
    }  
}

</script>
