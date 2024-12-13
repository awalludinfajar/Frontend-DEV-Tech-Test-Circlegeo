<script setup>
import Map from '@/pages/components/baseMap';
import polygonLayer from './components/polygonLayer.vue';
import lineLayer from './components/lineLayer.vue';
import markerLayer from './components/markerLayer.vue';
import circleLayer from './components/circleLayer.vue';

</script>

<template>
    <div class="flex flex-col min-h-screen bg-gray-100">
        <main class="flex-grow py-6">
            <Map>
                <lineLayer />
                <circleLayer />
                <markerLayer />
                <polygonLayer />
            </Map>
        </main>
    </div>
</template>