<script setup>
import Image from './components/Image.vue';
import { ref, onMounted, useTemplateRef } from 'vue'

const imageName = ref("./test_image.jpg");
const canvasReference = "canvasReferenceName"
const markers = ref([
    {
        markerLocationX: 50, 
        markerLocationY: 50,  
        title: "marker_title", 
        canvasStyle: undefined,//{shape:"circle", size:[20], color:"black"},
        canvasCode: undefined
    },
    {
        markerLocationX: 70, 
        markerLocationY: 70,  
        title: "marker_title_1", 
        canvasStyle: undefined,//{shape:"circle", size:[20], color:"black"},
        canvasCode: "he"
    }
]);

function canvasCodeFunction(){
    // const templateCanvas = useTemplateRef(canvasReference);
    console.log("Inside canvasCodeFunction function");
    return;
};
</script>

<template>
    <Image 
        :imageName="imageName"
        :markers="markers"
        :canvasReference="canvasReference"
        @canvasCodeFunction="canvasCodeFunction()"
    >
    </Image> 
</template>