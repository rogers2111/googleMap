<template>
    <div id=container>
      
      <div id="map"></div>
      <div id="searchdiv">
        <input type="text" id="pac-input" />
        </div>
    </div>  
</template>

<script>
import gmapsInit from './utils/gmaps';
import 'regenerator-runtime/runtime';
export default {
  name: 'App',

  data() {
      return {
            map: null,
            google: null
      }
  },
  async mounted() {
      try {
          this.google = await gmapsInit();
          this.map = new this.google.maps.Map(document.getElementById("map"), { zoom: 8, center: {lat: 31.717831, lng: -84.247732}});
          this.searchBox();
      } catch ( error ) {
          console.log(error);
      }
  },
  methods: {
    searchBox(){
      const input = document.getElementById("pac-input");
      const searchBox = new this.google.maps.places.SearchBox(input);
      this.map.controls[this.google.maps.ControlPosition.TOP_LEFT].push(input);
      // Bias the SearchBox results towards current map's viewport.
      this.map.addListener("bounds_changed", () => {
        searchBox.setBounds(this.map.getBounds());
      });
      let markers = [];
      // Listen for the event fired when the user selects a prediction and retrieve
      // more details for that place.
      searchBox.addListener("places_changed", () => {
        const places = searchBox.getPlaces();

        if (places.length == 0) {
          return;
        }
        // Clear out the old markers.
        markers.forEach(marker => {
          marker.setMap(null);
        });
        markers = [];
        // For each place, get the icon, name and location.
        const bounds = new this.google.maps.LatLngBounds();
        places.forEach(place => {
          if (!place.geometry) {
            console.log("Returned place contains no geometry");
            return;
          }
          const icon = {
            url: place.icon,
            size: new this.google.maps.Size(71, 71),
            origin: new this.google.maps.Point(0, 0),
            anchor: new this.google.maps.Point(17, 34),
            scaledSize: new this.google.maps.Size(25, 25)
          };
          // Create a marker for each place.
          markers.push(
            new this.google.maps.Marker({
              map: this.map,
              icon,
              title: place.name,
              position: place.geometry.location
            })
          );

          if (place.geometry.viewport) {
            // Only geocodes have viewport.
            bounds.union(place.geometry.viewport);
          } else {
            bounds.extend(place.geometry.location);
          }
        });
        this.map.fitBounds(bounds);
      });
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
#container{
  position: relative;
}
#searchdiv{
  position: absolute;
  top:10px;
  left:50%;
}
#map {
  width: 100vw;
  height: 100vh;
}
</style>