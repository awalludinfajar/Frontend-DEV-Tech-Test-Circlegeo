<script>
export default {
    name: "PolygonLayer",
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
                this.map.addSource('polygon-layer-source', {
                    type: "geojson",
                    data: this.geojson,
                });

                this.map.addLayer({
                    id: "polygon-layer",
                    type: "fill",
                    source: "polygon-layer-source",
                    filter: ["==", "$type", "Polygon"],
                    paint: {
                        "fill-color": [
                            "case",
                            ["has", "fillColor"],
                            ["get", "fillColor"],
                            "#00BCD4",
                        ],
                        "fill-opacity": 0.5,
                    }
                });

                this.map.addLayer({
                    id: "polygon-outter",
                    type: "line",
                    source: "polygon-layer-source",
                    filter: ["==", "$type", "Polygon"],
                    paint: {
                        "line-color": [
                            "case",
                            ["has", "lineColor"],
                            ["get", "lineColor"],
                            "#00BCD4",
                        ],
                    }
                });
            });
        }
    }
}
</script>

<template>
</template>
