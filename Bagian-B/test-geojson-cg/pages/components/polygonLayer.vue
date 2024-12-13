<script setup>
import { inject, onMounted, watchEffect } from 'vue';
import geojsonData from '~/public/test.json';

const map = inject('map');

watchEffect(async() => {
  if (!map?.value) {
    return;
  }

  map.value.on('load', () => {
    if (!map.value.getSource('circle-source')) {
      const featureCollection = {
        type: 'FeatureCollection',
        features: [geojsonData.features[2]],
      };

      map.value.addSource('circle-source', {
        type: 'geojson',
        data: featureCollection,
      });

      map.value.addLayer({
        id: 'circle-layer',
        type: 'circle',
        source: 'circle-source',
        paint: {
          'circle-radius': ['get', 'radius'],
          'circle-color': '#FF5733',
          'circle-opacity': 0.6,
        },
      });
    }
  });
});
</script>

<template>
  <div></div>
</template>
