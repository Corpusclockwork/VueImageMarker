<script setup>
import Marker from './Marker.vue';
    const props = defineProps({
        imageName: {
            type: String,
            required: true
        },
        markers: {
            type: Set,
            default: new Set()
        },
        addMarkersOnClick: {
            type: Boolean,
            default: false
        },
    });
</script>
<template>
    <img :src="props.imageName"/>
    <div v-for="marker in markers">
        <Marker
            :markerId = marker.id
            :markerX = marker.x
            :markerY = marker.y
            :markerVisualProperties =marker.markerVisualProperties
        />
    </div>
</template>