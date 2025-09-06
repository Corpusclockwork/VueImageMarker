<script setup>
    import Marker from './Marker.vue';
    const props = defineProps({
        imageName: {
            type: String,
            required: true
        },
        markers: {
            type: Array,
            default: []
        },
        addMarkersOnClick: {
            type: Boolean,
            default: false
        },
    });
</script>
<template>
    <div class="imageContainer">
        <img 
            class="image"
            :src="props.imageName"
        />
        <Marker
            v-for="marker in props.markers"
            :key="marker.id"
            :markerId = marker.id
            :markerX = marker.x
            :markerY = marker.y 
            :title="marker.title"
        />
    </div>
</template>
<style>
.imageContainer{
    position: relative
}
</style>