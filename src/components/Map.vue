<template>
  <div id="container"></div>
</template>

<script>
import { Feature, Map, View } from "ol";
import { Point } from "ol/geom";
import TileLayer from "ol/layer/Tile";
import VectorLayer from "ol/layer/Vector";
import "ol/ol.css";
import { fromLonLat } from "ol/proj";
import { XYZ } from "ol/source";
import VectorSource from "ol/source/Vector";
import Icon from "ol/style/Icon";
import Style from "ol/style/Style";
export default {
  name: "OlMap",
  data() {
    return {
      map: null,
    };
  },

  mounted() {
    this.initMap();
    this.$nextTick(() => this.renderManyPoints());
  },
  methods: {
    initMap() {
      this.map = new Map({
        target: "container",
        controls: [],
        layers: [
          new TileLayer({
            source: new XYZ({
              url: "http://t0.tianditu.gov.cn/vec_w/wmts?SERVICE=WMTS&REQUEST=GetTile&VERSION=1.0.0&LAYER=vec&STYLE=default&TILEMATRIXSET=w&FORMAT=tiles&TILEMATRIX={z}&TILEROW={y}&TILECOL={x}&tk=936f87830ee440a2ee6948c7268613a5",
            }),
          }),
        ],
        view: new View({
          center: fromLonLat([113.639655, 22.715068]),
          zoom: 13,
          maxZoom: 18,
          minZoom: 4,
        }),
      });
    },

    renderManyPoints() {
      fetch("http://47.108.176.152:8881/api/geo/live/all")
        .then((res) => res.json())
        .then((result) => {
          const data = Object.values(result)
            .map((val) => Array.isArray(val) && val)
            .flat();

          const features = data.map((d) => {
            const feature = new Feature(new Point(fromLonLat([d.lon, d.lat])));
            return feature;
          });

          const layer = new VectorLayer({
            source: new VectorSource({
              features,
            }),
            style: function () {
              return new Style({
                image: new Icon({
                  src: require("@/assets/ship.png"),
                  width: 20,
                  height: 20,
                }),
              });
            },
          });
          this.map.addLayer(layer);
        });
    },
  },
};
</script>

<style>
#container {
  width: 100%;
  height: 100%;
}
</style>
