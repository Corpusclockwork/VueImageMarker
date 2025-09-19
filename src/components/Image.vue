<script setup>
    import { ref, onMounted, useTemplateRef, watch, render, onUpdated, toRaw } from 'vue'
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
        canvasReference: {
            type: String,
            required: true
        },
        addMarkersOnClick: {
            type: Boolean,
            default: false
        },
    });
    const emit = defineEmits(['callCanvasCodeFunction'])

    const imageWidth = ref(0);
    const imageHeight = ref(0);    
    const templateCanvas = useTemplateRef(props.canvasReference);

    function calculateMarkerXPosition(percentageAcross, markerWidth){
        // divide the marker width by 2 so that the marker is centered at the correct point
        console.log(imageWidth.value*(percentageAcross/100) - (markerWidth/2));
        return imageWidth.value*(percentageAcross/100) - (markerWidth/2); 
    }
    function calculateMarkerYPosition(percentageDown, markerHeight){
        // divide the marker height by 2 so that the marker is centered at the correct point
        console.log(imageHeight.value*(percentageDown/100) - (markerHeight/2));
        return imageHeight.value*(percentageDown/100) - (markerHeight/2);
    }
    function calculateMarkerWidth(markerWidth, isCurved){
        if (isCurved){
            return (imageWidth.value*(markerWidth/100))/2;
        }
        console.log(imageWidth.value*(markerWidth/100));
        return imageWidth.value*(markerWidth/100);
    }
    function calculateMarkerHeight(markerHeight){
        console.log(imageWidth.value*(markerHeight/100));
        return imageWidth.value*(markerHeight/100);
    }

    function drawMarkers(){
        var ctx = templateCanvas.value.getContext("2d");
        // now, just repeat this for all the markers 
        for (let marker of props.markers){
            if (marker.canvasCode) {
                // emit the function name
                emit('callCanvasCodeFunction');
            }
            // honestly tempted to get rid of the canvasStyle choices, and just have to canvasCode function call.
            // any choices I have are just not going to cover all the cases that one may want, and writing real canvas code seems less complicated
            // than writing for the specifications I have made  
            else if (marker.canvasStyle){
                const shape = new Path2D();
                switch (marker.canvasStyle.shape) {
                    case "circle":
                        radius = calculateMarkerWidth(marker.canvasStyle.size[0], true);
                        startAngle = 0;
                        endAngle = 2 * Math.PI;
                    case "ellipse":
                        radiusX = calculateMarkerWidth(marker.canvasStyle.size[0], true);
                        radiusY = calculateMarkerHeight(marker.canvasStyle.size[1], false);
                        startAngle = 0;
                        endAngle = 2 * Math.PI;
                    case "square":
                        width = calculateMarkerWidth(marker.canvasStyle.size[0], false);
                    case "rectangle":
                        width = calculateMarkerWidth(marker.canvasStyle.size[0], false);
                        height = calculateMarkerHeight(marker.canvasStyle.size[1], false);
                    case "triangle_up":
                        width = calculateMarkerWidth(marker.canvasStyle.size[0], false);
                        height = calculateMarkerHeight(marker.canvasStyle.size[1], false);
                    case "triangle_down":
                        width = calculateMarkerWidth(marker.canvasStyle.size[0], false);
                        height = calculateMarkerHeight(marker.canvasStyle.size[1], false);
                    case "star":
                        width = calculateMarkerWidth(marker.canvasStyle.size[0], false);
                        height = calculateMarkerHeight(marker.canvasStyle.size[1], false);
                }
                color = marker.canvasStyle.color;
            } else {
                // default marker shape, size, and colour, is a red circle that has a width of 10%
                ctx.strokeStyle = "red";
                const shape = new Path2D();
                shape.arc(
                    calculateMarkerXPosition(marker.markerLocationX,10),
                    calculateMarkerYPosition(marker.markerLocationY,10),
                    calculateMarkerWidth(10, true),
                    0, 
                    2 * Math.PI
                );
                ctx.stroke(shape);
                ctx.fillStyle = "red";
                ctx.fill(shape);
                ctx.fillText("Hello World", 600, 500)
            }
        }
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
            :ref=canvasReference
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