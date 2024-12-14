<script>
import { Marker } from 'maplibre-gl';
import maplibregl from 'maplibre-gl';

export default {
    name: 'DefaultMarkerLayer',
    props: {
        map: {
            type: Object,
            required: true,
        },
        geojson: {
            type: Object,
            required: true,
        },
    },
    mounted() {
        if (this.map && this.geojson) {
            this.map.on("style.load", () => {
                this.geojson.features
                .filter((feature) => 
                    feature.geometry.type === "Point" && 
                    feature.properties.name === "Marker Default")
                .forEach(feature => {
                    const { coordinates } = feature.geometry;
                    const { name } = feature.properties;

                    const marker = new Marker()
                    .setLngLat(coordinates)
                    .setPopup(new maplibregl.Popup().setText(name || "Default Marker"))
                    .addTo(this.map);
                });
            });
        }
    }
};
</script>

<template>
</template>