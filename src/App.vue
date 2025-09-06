<script setup>
import Image from './components/Image.vue';
import { ref, onMounted, useTemplateRef } from 'vue'

// 1. mount everything 
// 2. Go through the mounted marker and use template ref to style them /
// 3. Profit  ?

const imageName = ref("./alien_bloc_logo.jpg");

function markerCanvas(markerRef) {
    const templateCanvas = useTemplateRef(markerRef);
    var ctx = templateCanvas.value.getContext("2d");
    ctx.beginPath();
    ctx.rect(0, 0, 150, 100);
    ctx.stroke();
}

onMounted(() => {
    markerCanvas("marker-1") // do this for all markers ?
})

const markers = ref([{
    id: 1, 
    x: 50, 
    y: 50,  
    title:""
}]);


</script>

<template>
    <Image 
        :imageName="imageName"
        :markers="markers"
    >
    </Image> 
</template>

<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}
.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}
.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}
</style>
