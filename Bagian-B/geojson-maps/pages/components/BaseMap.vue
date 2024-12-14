<script>
import { Map } from 'maplibre-gl';
import geoData from '~/public/test.json';
import { shallowRef, onMounted, onUnmounted, markRaw } from 'vue';
import LineLayer from './Layers/LineLayer.vue';
import PolygonLayer from './Layers/PolygonLayer.vue';
import DefaultMarkerLayer from './Layers/DefaultMarkerLayer.vue';
import CircleLayer from './Layers/CircleLayer.vue';
import CustomImageMarkerLayer from './Layers/CustomImageMarkerLayer.vue';

export default {
    name: "BaseMap",
    components: {
        LineLayer,
        PolygonLayer,
        DefaultMarkerLayer,
        CircleLayer,
        CustomImageMarkerLayer
    },
    setup () {
        const mapContainer = shallowRef(null);
        const map = shallowRef(null);

        onMounted(() => {
            const initialContry = {lng: 120, lat: -5, zoom: 4};
            if (mapContainer.value) {                
                map.value = markRaw(new Map({
                    container: mapContainer.value,
                    style: 'https://demotiles.maplibre.org/style.json',
                    center: [initialContry.lng, initialContry.lat],
                    zoom: initialContry.zoom
                }));
            }
        }), 
        onUnmounted(() => {
            map.value?.remove();
        })

        return {
            map, mapContainer, geoData
        };
    }
};
</script>

<template>
    <div class="map-wrap">
        <div class="map" ref="mapContainer"></div>
        <LineLayer v-if="map" :map="map" :geojson="geoData" />
        <PolygonLayer v-if="map" :map="map" :geojson="geoData" />
        <DefaultMarkerLayer v-if="map" :map="map" :geojson="geoData" />
        <CircleLayer v-if="map" :map="map" :geojson="geoData" />
        <CustomImageMarkerLayer v-if="map" :map="map" :geojson="geoData" />
    </div>
</template>

<style scoped>
@import 'maplibre-gl/dist/maplibre-gl.css';

.map-wrap {
  position: relative;
  width: 100%;
  height: calc(100vh - 77px); /* calculate height of the screen minus the heading */
}

.map {
  position: absolute;
  width: 100%;
  height: 100%;
}

.watermark {
  position: absolute;
  left: 10px;
  bottom: 10px;
  z-index: 999;
}
</style>