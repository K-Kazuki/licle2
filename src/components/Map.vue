<template>
  <div class="mapPane">
    <!--マップ-->
    <l-map :zoom="zoom" :center="center" :options="{zoomControl: false}">

      <!--レイヤ設定-->
      <l-tile-layer
        :key="tileProvider.name"
        :name="tileProvider.name"
        :visible="tileProvider.visible"
        :url="tileProvider.url"
        :attribution="tileProvider.attribution"
        layer-type="base"
      ></l-tile-layer>

      <!--マーカー-->
      <l-marker :lat-lng="marker"></l-marker>

      <!-- circle -->
      <l-circle
      v-for="circle in circles"
      :key="circle.id"
      :lat-lng="circle.center"
      :radius="circle.radius"
      :color="circle.color"
      :fillColor="circle.fillColor"
      :fillOpacity="circle.fillOpacity"
    />
    </l-map>

    <button @click="getCurrentLocation">getCurrentLocation</button>
  </div>

</template>

<script>
import { LMap, LTileLayer, LMarker, LCircle } from 'vue2-leaflet'

export default {
  name: 'Map',
  components: {
    LMap,
    LTileLayer,
    LMarker,
    LCircle
  },
  data () {
    return {
      center: [35.681, 139.763],
      zoom: 14,
      marker: [35.681, 139.763],
      tileProvider: {
        name: '国土地理院',
        visible: true,
        url: 'https://cyberjapandata.gsi.go.jp/xyz/std/{z}/{x}/{y}.png',
        attribution: 'https://cyberjapandata.gsi.go.jp'
      },
      circles: [
        {
          id: 'circle-1',
          center: [35.681, 139.763],
          radius: 4500,
          color: '#F4442E',
          fillColor: '#F4442E',
          fillOpacity: 0.4
        }
      ]
    }
  },
  methods: {
    getCurrentLocation: function () {
      navigator.geolocation.getCurrentPosition(function (location) {
        const center = [location.coords.latitude, location.coords.longitude]
        this.center = center
        this.marker = center
      }.bind(this))
    }
  }
}
</script>

<style scoped>
.mapPane {
  height: 600px;
  margin: 0;
  text-align: left;
}
</style>
