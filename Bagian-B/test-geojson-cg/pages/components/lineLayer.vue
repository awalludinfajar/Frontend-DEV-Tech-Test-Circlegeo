<script setup>
import { inject, watchEffect } from 'vue';
import maplibregl from 'maplibre-gl';
import geojsonData from '~/public/test.json';

const map = inject('map');

watchEffect(async() => {
  if (!map?.value) {
    return;
  }

  map.value.on('load', () => {
    if (!map.value.getSource('lines-source')) {
      const featureCollection = {
        type: 'FeatureCollection',
        features: [geojsonData.features[5]],
      };

      map.value.addSource('lines-source', {
        type: 'geojson',
        data: featureCollection,
      });

      map.value.addLayer({
        id: 'lines-layer',
        type: 'line',
        source: 'lines-source',
        paint: {
          'line-color': ['get', 'lineColor'],
          'line-width': 3,
          'line-opacity': 0.8,
        },
      });

      map.value.on('click', 'lines-layer', (e) => {
        const features = map.value.queryRenderedFeatures(e.point, {
          layers: ['lines-layer'],
        });

        if (!features.length) return;

        const feature = features[0];
        const geometryType = feature.geometry.type;
        const coordinates = geometryType === 'LineString' 
          ? feature.geometry.coordinates[Math.floor(feature.geometry.coordinates.length / 2)] : feature.geometry.coordinates[0][0];
        const properties = feature.properties;

        new maplibregl.Popup()
          .setLngLat(coordinates)
          .setHTML(`
            <div>
              <strong>Line Color:</strong> ${properties.lineColor || 'No Color'}
            </div>
          `)
          .addTo(map.value);
      });

      map.value.on('mouseenter', 'lines-layer', () => {
        map.value.getCanvas().style.cursor = 'pointer';
      });

      map.value.on('mouseleave', 'lines-layer', () => {
        map.value.getCanvas().style.cursor = '';
      });
    }
  });
});
</script>

<template>
  <div></div>
</template>
