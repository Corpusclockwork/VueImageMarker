<script setup>
import HelloWorld from './components/HelloWorld.vue'
import Image from './components/Image.vue';
import { ref, onMounted } from 'vue'

const imageName = ref("./alien_bloc_logo.jpg");
const markers = ref(new Set());
</script>

<template>
    <Image 
        :imageName="imageName"
        :markers="markers"
    ></Image>
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
