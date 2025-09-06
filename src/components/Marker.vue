<script setup>
    import { ref, onMounted, computed, useTemplateRef} from 'vue'

    const props = defineProps({
        markerId: Number,
        markerX: Number,
        markerY: Number,
        markerCanvas: Function,
        markerVisualProperties: Object
    })

    const templateCanvas = useTemplateRef("canvas");
    
    onMounted(() => {
        var ctx = templateCanvas.value.getContext("2d");
        props.markerCanvas(ctx)
        // ctx.beginPath();
        // ctx.rect(0, 0, 150, 100);
        // ctx.stroke();
    });

    const calculateTop = computed(() => {
        return (props.markerX - props.markerVisualProperties.height/2 ) + '%';
    });

    const calculateLeft = computed(() => {
        return (props.markerY -  props.markerVisualProperties.width/2) + '%';
    });

    const calculateWidth = computed(() => {
        return props.markerVisualProperties.width + "%";
    });

    const calculateHeight = computed(() => {
        return props.markerVisualProperties.height  + "%";
    });

</script>
<template>
    <div
        :id="markerId"
        class="markerClass"
    >
        <canvas ref="marker-1"></canvas>
    </div>
</template>
<style>
.markerClass {
    position: absolute;
    background-color: red;
    color: red;
    width: v-bind(calculateWidth);
    height: v-bind(calculateHeight);
    top: v-bind(calculateTop);
    left: v-bind(calculateLeft);
}
</style>