<template>
  <div id="app">
    <yandex-map
      zoom="11"
      style="width: 100%; max-width: 100%; height: 98vh"
      :coords="coords"
      @click="onClick"
      @map-was-initialized="initMap"
    >
      <ymap-marker
        :coords="coords"
        marker-id="123"
        hint-content="some hint"
      ></ymap-marker>
    </yandex-map>
  </div>
</template>

<script>
import { yandexMap, ymapMarker, loadYmap } from "vue-yandex-maps";
import mkadCoords from "./mkad";

export default {
  components: { yandexMap, ymapMarker },

  data() {
    this.globalMap = null;
    this.clickedCoords = null;
    this.closestCoords = null;
    this.coords = [55.753994, 37.622093];
    this.myCollection = null;
    this.mkadCoords = mkadCoords;
  },
  methods: {
    async mounted() {
      await loadYmap();
    },

    onClick(el) {
      this.clickedCoords = el.get("coords");
      // eslint-disable-next-line no-undef

      // var closestObject = this.clickedCoords.getClosestTo(this.globalMap);
      // console.log(closestObject);

      // let closestObject = this.myCollection.getClosestTo(this.clickedCoords);
      // eslint-disable-next-line no-undef
      let newMark = new ymaps.Placemark(this.clickedCoords);
      window.ymaps.geoQuery(newMark).addToMap(this.globalMap);
      // var closestObject = this.myCollection.getClosestTo(newMark);
      console.log(newMark);
      // console.log(closestObject);
      //   console.log(this.clickedCoords);
    },

    initMap(map) {
      this.globalMap = map;
      this.addCoordinates();
    },

    setCollection() {
      // eslint-disable-next-line no-undef
      this.myCollection = new ymaps.GeoObjectCollection(
        {},
        {
          visible: true,
        }
      );
    },

    addCoordinates() {
      this.setCollection();
      for (var i = 0; i < this.mkadCoords.length; i++) {
        // eslint-disable-next-line no-undef
        this.myCollection.add(new ymaps.Placemark(this.mkadCoords[i]));
        this.globalMap.geoObjects.add(this.myCollection);
      }
    },
  },
};
</script>

<style>
</style>