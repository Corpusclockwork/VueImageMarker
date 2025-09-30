# Vue Image Marker

I made a Vue equivalent of this (https://www.npmjs.com/package/react-image-marker) (or an alternative version of this (https://github.com/jarvisniu/vue-image-marker) I suppose) for one of my other projects. The project is structured using Vue Composition API, which is apparently the way forward when it comes to Vue. I had never used it before so built this to get some experience in it. 

## Functionality
The component is called ImageMarker, and it takes the neccessary props, imageName, canvasReference and markers. In the parent component you are using the ImageMarker in, you can use the functions               `calculateMarkerXPositionRect`, 
`calculateMarkerYPositionRect`, 
`calculateMarkerXPositionArc`,
`calculateMarkerYPositionArc`,
`calculateMarkerWidthRect`, 
`calculateMarkerHeightRect`,
`calculateMarkerWidthArc`, and
`calculateMarkerHeightArc`. 
You use these functions in the main component to  react to changes in the image size. In the component where the ImageMarker is being used, define the function canvasCodeFunction, which should loop through all the markers and define what each marker should look like. Here is an example: 
```
function canvasCodeFunction(ctx){
    for (let marker of markers.value){
        const shape = new Path2D();
        shape.rect(
            imageMarker.value.calculateMarkerXPositionRect(marker.markerLocationX,marker.markerWidth),
            imageMarker.value.calculateMarkerYPositionRect(marker.markerLocationY,marker.markerHeight),
            imageMarker.value.calculateMarkerWidthRect(marker.markerWidth + size),
            imageMarker.value.calculateMarkerHeightRect(marker.markerHeight + size)
        )
        ctx.strokeStyle = "red";
        ctx.stroke(shape);
    }
};
```
This will make all the markers red rectangles of the given width and height.
## Adding Markers
To add markers on click, you can use the addMarkersOnClick prop, and then specify the width and height of new Markers using newMarkersWidth, newMarkersHeight. You then need to deine a function in the parent app that you call when a user clicks on the canvas, which I have called AddNew Marker. This function then calls the canvasCodeFunction.

```
function addNewMarker(markerX, markerY, ctx){
    markers.value.push({
        markerLocationX: markerX, 
        markerLocationY: markerY,  
        markerWidth: 5,
        markerHeight: 5
    });
    canvasCodeFunction(ctx)
};
```

When calling ImageMarker, you also need to make sure that you include addMarkersOnClick to be true (the default is false), and also that you include the onClick emit to call your addNewMarker function when a new marker needs to be added.
```
:addMarkersOnClick="true"
@onClick="addNewMarker"
```

## Examples

Here are some example marker designs on a genric image:
### Smiley face 
<img width="1052" height="703" alt="smiley_faces" src="https://github.com/user-attachments/assets/e4980301-f87c-492a-9c08-d4181c759bef" />
The function to draw the markers is:
```
function canvasCodeFunction(ctx){
    for (let marker of markers.value){
        const shape = new Path2D();
         shape.arc(
            imageMarker.value.calculateMarkerXPositionArc(marker.markerLocationX,marker.markerWidth),
            imageMarker.value.calculateMarkerYPositionArc(marker.markerLocationY,marker.markerHeight),
            imageMarker.value.calculateMarkerWidthArc(marker.markerWidth),
            0,
            2 * Math.PI
        );
        ctx.strokeStyle = "black";
        ctx.stroke(shape);
        ctx.fillStyle = "yellow";
        ctx.fill(shape); 

        const eye1 = new Path2D();
        eye1.arc(
            imageMarker.value.calculateMarkerXPositionArc(marker.markerLocationX - 2,marker.markerWidth),
            imageMarker.value.calculateMarkerYPositionArc(marker.markerLocationY - 2,marker.markerHeight),
            imageMarker.value.calculateMarkerWidthArc(marker.markerWidth - 8),
            0,
            2 * Math.PI
        );
        ctx.stroke(eye1);
        ctx.fillStyle = "black";
        ctx.fill(eye1);

        const eye2 = new Path2D();
        eye2.arc(
            imageMarker.value.calculateMarkerXPositionArc(marker.markerLocationX + 2,marker.markerWidth),
            imageMarker.value.calculateMarkerYPositionArc(marker.markerLocationY - 2,marker.markerHeight),
            imageMarker.value.calculateMarkerWidthArc(marker.markerWidth - 8),
            0,
            2 * Math.PI
        );
        ctx.stroke(eye2);
        ctx.fillStyle = "black";
        ctx.fill(eye2);
    }
};
```
### Star 
### Different colour circles
