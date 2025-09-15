<script setup>
    import { ref, onMounted, useTemplateRef, watch, render, onUpdated } from 'vue'
    import { vResizeObserver } from '@vueuse/components'
    const props = defineProps({
        imageName: {
            type: String,
            required: true
        },
        markers: {
            type: Array,
            default: [],
            required: true
        },
        addMarkersOnClick: {
            type: Boolean,
            default: false
        },
    });

    const imageWidth = ref(0);
    const imageHeight = ref(0);    
  
    const templateCanvas = useTemplateRef("canvas");

     function drawMarkers(){
        var ctx = templateCanvas.value.getContext("2d");
        // now, just repeat this for all the markers 
        ctx.strokeStyle = 'black'
        const rectangle = new Path2D();
        rectangle.rect(imageWidth.value*(30/100) -25, imageHeight.value*(30/100)-25, 50, 50);
        ctx.stroke(rectangle);
    }

    // I want to resize based on the actual image, not the window size, so it seems better to use this rather than a Window: resize event event
    function onResizeObserver(entries){
        const [entry] = entries
        const {width, height} = entry.contentRect;
        imageWidth.value = width;
        imageHeight.value = height;
    };

    onMounted(() => {
        drawMarkers();
    });

    onUpdated(() => {
        drawMarkers();
    });

</script>
<template>
    <div class="ImageMarkerContainer">
        <div class="imageContainer">
            <img v-resize-observer="onResizeObserver"
                id="imageID"
                class="image"
                ref="image"
                :src="props.imageName"
            />
        </div>
        <canvas 
            ref="canvas"
            class="canvasClass"
            :width=imageWidth
            :height=imageHeight
        ></canvas>
    </div>
</template>
<style>
.ImageMarkerContainer{
    position: relative;
}

.imageContainer{
    position: absolute;
}

.image{
    width: 80vw;
}

.canvasClass {
    background: v-bind(imageUrl);
    position: absolute;
}
</style>