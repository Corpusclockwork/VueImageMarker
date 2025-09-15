<script setup>
import Image from './components/Image.vue';
import { ref, onMounted, useTemplateRef } from 'vue'

const imageName = ref("./alien_bloc_logo.jpg");
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