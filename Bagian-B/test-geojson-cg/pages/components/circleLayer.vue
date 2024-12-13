<script setup>
import { inject, watchEffect } from 'vue';
import geojsonData from '~/public/test.json';

const map = inject('map');

watchEffect(async() => {
  if (!map?.value) {
    return;
  }

  map.value.on('load', () => {
    if (!map.value.getSource('polygon-source')) {
      const featureCollection = {
        type: 'FeatureCollection',
        features: [geojsonData.features[4]],
      };

      map.value.addSource('polygon-source', {
        type: 'geojson',
        data: featureCollection,
      });

      map.value.addLayer({
        id: 'polygon-layer',
        type: 'fill',
        source: 'polygon-source',
        paint: {
          'fill-color': ['get', 'fillColor'],
          'fill-opacity': 0.5,
          'fill-outline-color': ['get', 'lineColor'],
        },
      });
    }
  });
});
</script>

<template>
  <div></div>
</template>
