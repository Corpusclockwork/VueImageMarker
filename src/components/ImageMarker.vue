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
            required: true
        },
        addMarkersOnClick: {
            type: Boolean,
            default: false
        },
    });

    defineExpose({
        calculateMarkerXPositionRect, 
        calculateMarkerYPositionRect, 
        calculateMarkerXPositionArc,
        calculateMarkerYPositionArc,
        calculateMarkerWidthRect, 
        calculateMarkerHeightRect,
        calculateMarkerWidthArc,
        calculateMarkerHeightArc
    });
    
    const emit = defineEmits(['callCanvasCodeFunction', 'onClick'])

    const imageWidth = ref(0);
    const imageHeight = ref(0);    
    const templateCanvas = useTemplateRef(props.canvasReference);

    function calculateMarkerXPositionRect(percentageAcross, markerWidth){
        return imageWidth.value*((percentageAcross/100) - (markerWidth/200)); 
    }
    function calculateMarkerYPositionRect(percentageDown, markerHeight){
        return imageHeight.value*((percentageDown/100)- (markerHeight/200));
    }
    function calculateMarkerXPositionArc(percentageAcross){
        return imageWidth.value*((percentageAcross/100)); 
    }
    function calculateMarkerYPositionArc(percentageDown){
        return imageHeight.value*((percentageDown/100));
    }
    function calculateMarkerWidthRect(markerWidth){
        return imageWidth.value*(markerWidth/100);
    }
    function calculateMarkerHeightRect(markerHeight){
        return imageHeight.value*(markerHeight/100);
    }
    function calculateMarkerWidthArc(markerWidth){
        return (imageWidth.value*(markerWidth/100))/2;
    }
    function calculateMarkerHeightArc(markerHeight){
        return (imageHeight.value*(markerHeight/100))/2;
    }

    function drawMarkers(){
        const ctx = templateCanvas.value.getContext("2d");
        emit('callCanvasCodeFunction', ctx);
    }

    function addMarker(event){
        if(!props.addMarkersOnClick){
            return;
        }
        const newMarkerX = (event.offsetX / imageWidth.value) * 100;
        const newMarkerY = (event.offsetY / imageHeight.value) * 100;
        const ctx = templateCanvas.value.getContext("2d");
        emit('onClick', newMarkerX, newMarkerY, ctx);
    };
    
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
            @click="addMarker"
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