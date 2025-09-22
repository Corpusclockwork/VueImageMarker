<script setup>
import ImageMarker from './components/ImageMarker.vue';
import { ref, onMounted, useTemplateRef } from 'vue'

const imageMarker = ref(null);
const imageName = "./test_image.jpg";
const canvasReference = "canvasReferenceName";
const markers = ref([
    {
        markerLocationX: 0, 
        markerLocationY: 0,  
        markerWidth: 10,
        markerHeight: 10
    },
    {
        markerLocationX: 70, 
        markerLocationY: 70,  
        markerWidth: 10,
        markerHeight: 10
    }
]);

function canvasCodeFunction(ctx){
    for (let marker of markers.value){
        const shape = new Path2D();
        shape.arc(
            imageMarker.value.calculateMarkerXPosition(marker.markerLocationX,10),
            imageMarker.value.calculateMarkerYPosition(marker.markerLocationY,10),
            imageMarker.value.calculateMarkerWidthForCurvedShape(marker.markerWidth),
            0, 
            2 * Math.PI
        );
        ctx.strokeStyle = "red";
        ctx.stroke(shape);
        ctx.fillStyle = "red";
        ctx.fill(shape);
    }
};
</script>

<template>
    <ImageMarker 
        ref="imageMarker"
        :imageName="imageName"
        :canvasReference="canvasReference"
        :markers="markers"
        @callCanvasCodeFunction="canvasCodeFunction"
    >
    </ImageMarker> 
</template>