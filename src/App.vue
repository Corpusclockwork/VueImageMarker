<script setup>
import ImageMarker from './components/ImageMarker.vue';
import { ref } from 'vue'

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
        shape.rect(
            imageMarker.value.calculateMarkerXPositionRect(marker.markerLocationX,marker.markerWidth),
            imageMarker.value.calculateMarkerYPositionRect(marker.markerLocationY,marker.markerHeight),
            imageMarker.value.calculateMarkerWidthRect(marker.markerWidth),
            imageMarker.value.calculateMarkerHeightRect(marker.markerHeight)
        )
        ctx.strokeStyle = "red";
        ctx.fillStyle = "red";
        ctx.fill(shape);
        ctx.stroke(shape);
    }
};

function addNewMarker(markerX, markerY, ctx){
    markers.value.push({
        markerLocationX: markerX, 
        markerLocationY: markerY,  
        markerWidth: 10,
        markerHeight: 10
    });
    canvasCodeFunction(ctx)
};
</script>
<template>
    <ImageMarker 
        ref="imageMarker"
        :imageName="imageName"
        :canvasReference="canvasReference"
        :markers="markers"
        :addMarkersOnClick="true"
        @onClick="addNewMarker"
        @callCanvasCodeFunction="canvasCodeFunction"
    >
    </ImageMarker> 
</template>