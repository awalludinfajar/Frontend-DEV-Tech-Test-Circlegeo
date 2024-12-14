<script>
import { Marker, Popup } from 'maplibre-gl';

export default {
    name: 'CircleLayer',
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
                    feature.properties.name === "Circle Marker")
                .forEach(feature => {
                    const { coordinates } = feature.geometry;
                    const { radius = 10, name } = feature.properties;
                    
                    const el = document.createElement("div");
                    el.style.width = `${radius * 2}px`;
                    el.style.height = `${radius * 2}px`;
                    el.style.backgroundColor = "rgba(0, 123, 255, 0.6)";
                    el.style.borderRadius = "50%";
                    el.style.border = "2px solid blue";
                    el.style.cursor = "pointer";
                    
                    new Marker({ element : el })
                    .setLngLat(coordinates)
                    .setPopup(new Popup().setText(name || "Circle Marker"))
                    .addTo(this.map);
                });
            });
        }
    }
}
</script>

<template>
</template>