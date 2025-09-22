# Vue Image Marker

I made a Vue equivalent of this (https://www.npmjs.com/package/react-image-marker) (or an alternative version of this (https://github.com/jarvisniu/vue-image-marker) I suppose) for one of my other projects. The project is structured using Vue Composition API, which is apparently the way forward when it comes to Vue. I had never used it before so built this to get some experince in it. 

## Functionality
The component is called ImageMarker, and it takes the neccessary props, imageName, canvasReference and markers. In the parent component you are using the ImageMarker in, you can use the functions calculateMarkerXPosition, calculateMarkerYPosition, calculateMarkerWidth, calculateMarkerHeight, and calculateMarkerWidthForCurvedShape. You use these functions in the main component to  react to changes in the image size. In the component where the ImageMarker is being used, define the function canvasCodeFunction, which should loop through all the markers and define what each marker should look like using a Path2D object. Here is an example: 
```
function canvasCodeFunction(ctx){
    for (let marker of markers.value){
        const shape = new Path2D();
        shape.rect(
            imageMarker.value.calculateMarkerXPosition(marker.markerLocationX, marker.markerWidth),
            imageMarker.value.calculateMarkerYPosition(marker.markerLocationY, marker.markerHeight),
            imageMarker.value.calculateMarkerWidth(marker.markerWidth),
            imageMarker.value.calculateMarkerWidth(marker.markerHeight)
        )
        ctx.strokeStyle = "red";
        ctx.stroke(shape);
    }
};
```
## Adding Markers
To add markers on click, you can use the addMarkersOnClick prop, and then specify the width and height of new Markers using newMarkersWidth, newMarkersHeight.
