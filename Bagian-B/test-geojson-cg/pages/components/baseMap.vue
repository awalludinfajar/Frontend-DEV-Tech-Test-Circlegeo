<script setup>
import { provide, ref, onMounted, nextTick } from 'vue';
import maplibregl from 'maplibre-gl';

const mapContainer = ref(null);
const map = ref(null);

onMounted(async () => {
	await nextTick();
  
	if (mapContainer.value) {
		map.value = new maplibregl.Map({
			container: mapContainer.value,
			style: 'https://demotiles.maplibre.org/style.json',
			center: [120, -5],
			zoom: 4,
		});

    	map.value.addControl(new maplibregl.NavigationControl(), 'top-right');
  	}
});

provide('map', map);
</script>

<template>
  <div ref="mapContainer" class="map-container">
    <slot />
  </div>
</template>

<style scoped>
.map-container {
  width: 100%;
  height: 900px;
}
</style>
