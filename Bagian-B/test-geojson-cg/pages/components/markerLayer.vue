<script setup>
import { inject, onMounted, watchEffect } from 'vue';
import maplibregl from 'maplibre-gl';
import geojsonData from '~/public/test.json';

const map = inject('map');

watchEffect(async() => {
  if (!map?.value) {
    return;
  }

  map.value.on('load', () => {
    if (!map.value.getSource('points-source')) {
      map.value.addSource('points-source', {
        type: 'geojson',
        data: {
          type: 'FeatureCollection',
          features: [geojsonData.features[0]],
        },
      });

      map.value.addLayer({
        id: 'points-layer',
        type: 'circle',
        source: 'points-source',
        paint: {
          'circle-radius': 8,
          'circle-color': '#FF5722',
          'circle-stroke-width': 2,
          'circle-stroke-color': '#FFFFFF',
        },
      });

      map.value.on('click', 'points-layer', (e) => {
        const features = map.value.queryRenderedFeatures(e.point, {
          layers: ['points-layer'],
        });

        if (!features.length) return;

        const feature = features[0];
        const coordinates = feature.geometry.coordinates.slice();
        const properties = feature.properties;

        while (Math.abs(e.lngLat.lng - coordinates[0]) > 180) {
          coordinates[0] += e.lngLat.lng > coordinates[0] ? 360 : -360;
        }

        new maplibregl.Popup()
          .setLngLat(coordinates)
          .setHTML(`<strong>${properties.name || 'No Name'}</strong>`)
          .addTo(map.value);
      });

      map.value.on('mouseenter', 'points-layer', () => {
        map.value.getCanvas().style.cursor = 'pointer';
      });

      map.value.on('mouseleave', 'points-layer', () => {
        map.value.getCanvas().style.cursor = '';
      });
    }
  });
});
</script>

<template>
  <div></div>
</template>
