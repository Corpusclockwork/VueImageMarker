<script setup>
    import { ref, onMounted, useTemplateRef, onUpdated } from 'vue'
    import { vResizeObserver } from '@vueuse/components'
    const props = defineProps({
        imageName: {
            type: String,
            required: true
        },
        canvasReference : {
            type: String,
            required: true
        },
        markers: {
            type: Array,
            default: [],
            required: true
        }
    });

    defineExpose({
        calculateMarkerXPosition, 
        calculateMarkerYPosition, 
        calculateMarkerWidth, 
        calculateMarkerHeight, 
        calculateMarkerWidthForCurvedShape
    });
    
    const emit = defineEmits(['callCanvasCodeFunction'])

    const imageWidth = ref(0);
    const imageHeight = ref(0);    
    const templateCanvas = useTemplateRef(props.canvasReference);

    function calculateMarkerXPosition (percentageAcross, markerWidth){
        // divide the marker width by 2 so that the marker is centered at the correct point
        return imageWidth.value*(percentageAcross/100) - (markerWidth/2); 
    }
    function calculateMarkerYPosition(percentageDown, markerHeight){
        // divide the marker height by 2 so that the marker is centered at the correct point
        return imageHeight.value*(percentageDown/100) - (markerHeight/2);
    }
    function calculateMarkerWidthForCurvedShape(markerWidth){
        return (imageWidth.value*(markerWidth/100))/2;
        
    }
    function calculateMarkerWidth(markerWidth){
        return imageWidth.value*(markerWidth/100);
    }
    function calculateMarkerHeight(markerHeight){
        return imageWidth.value*(markerHeight/100);
    }

    function drawMarkers(){
        var ctx = templateCanvas.value.getContext("2d");
        emit('callCanvasCodeFunction', ctx);
    }
    
    // I want to resize based on the actual image, not the window size, so it seems better to use this rather than a 'Window: resize' event
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
    <div class="imageMarkerCanvasContainerClass">
        <img 
            v-resize-observer="onResizeObserver"
            class="imageClass"
            :src="props.imageName"
        />
        <canvas 
            class="canvasClass"
            :ref=canvasReference
            :width=imageWidth
            :height=imageHeight
        ></canvas>
    </div>
</template>
<style>
.imageMarkerCanvasContainerClass{
    position: relative;
}

.imageClass{
    width: 100vw;
    position: absolute;
}

.canvasClass {
    position: absolute;
}
</style>