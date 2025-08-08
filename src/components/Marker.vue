<script setup>
import { ref, onMounted } from 'vue'

const props = defineProps({
    markerId: Number,
    markerX: Number,
    markerY: Number,
    markerVisualProperties: Object
})
function generateMarkerClassesObject() {
    classesObject = {}
    if (props.markerVisualProperties.shape === circle){
        classesObject['border-radius'] = '50%';
    }
    return classesObject
}
function generateMarkerStyleObject(){
    return {'left': markerX, 'top': markerY}
}
</script>
<template>
    <div
    :id="markerId"
    class="markerClass"
    :class="generateMarkerClassesObject"
    :style="generateMarkerStyleObject">
    </div>
</template>
<style>
.markerClass {
    position: absolute;
}
</style>