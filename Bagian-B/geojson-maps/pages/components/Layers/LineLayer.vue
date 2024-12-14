<script>
export default {
    name: 'LineLayer',
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
                this.map.addSource('line-layer-source', {
                    type: "geojson",
                    data: this.geojson,
                });
    
                this.map.addLayer({
                    id: "line-layer",
                    type: "line",
                    source: "line-layer-source",
                    filter: ["==", "$type", "LineString"],
                    paint: {
                        "line-color": [
                            "case",
                            ["has", "lineColor"],
                            ["get", "lineColor"],
                            "#FFC107",
                        ],
                        "line-width": 3,
                    },
                });
            });
        }
    },
};
</script>

<template>
</template>