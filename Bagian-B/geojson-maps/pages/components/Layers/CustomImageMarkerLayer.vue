<script>
import { Marker , Popup } from 'maplibre-gl';
import maplibregl from 'maplibre-gl';

export default {
    name: 'CustomImageMarkerLayer',
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

            const images = ['/Group-15-1024x253.png', '/tri.png'];
            let imageIndex = 0;

            this.geojson.features.forEach((marker) => {
                if ( marker.geometry.type === "Point" && marker.properties.name === "Custom Image Marker" ) {

                    const el = document.createElement('div');
                    el.style.backgroundImage = `url(${images[imageIndex]})`;
                    el.style.backgroundSize = 'cover';
                    el.style.width = `28px`;
                    el.style.height = `28px`;
                    el.style.cursor = 'pointer';

                    new maplibregl.Marker({ element: el })
                    .setLngLat(marker.geometry.coordinates)
                    .setPopup(new Popup().setText(name || "Image Marker"))
                    .addTo(this.map);

                    imageIndex++;
                }
            });
        }
    },
};
</script>

<template>
</template>

<style>
/* Optional: Style for the marker (in case of fallback) */
.marker {
    display: inline-block;
}
</style>
