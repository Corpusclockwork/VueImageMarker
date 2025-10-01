# Vue ImageMarker

I made a Vue equivalent of this (https://www.npmjs.com/package/react-image-marker) (or an alternative version of this (https://github.com/jarvisniu/vue-image-marker) I suppose) in one of my other projects (https://github.com/Corpusclockwork/alien_top_logger). I've remade this component using  Vue Composition API, which is apparently the way forwards when it comes to Vue. I had never used it before so built this to get some experience.

## Functionality
The component is called ImageMarker, and it takes the neccessary props, `imageName`, `canvasReference` and `markers`. In the parent component you are using the ImageMarker in, you can use the functions               `calculateMarkerXPositionRect`, 
`calculateMarkerYPositionRect`, 
`calculateMarkerXPositionArc`,
`calculateMarkerYPositionArc`,
`calculateMarkerWidthRect`, 
`calculateMarkerHeightRect`,
`calculateMarkerWidthArc`, and
`calculateMarkerHeightArc` to calculate the position and relative size of the markers based on the size of the image. The markers should change shape and size depending on the size of the image, so that they remain a consistent size in relation to the image size. In the component where the ImageMarker is being used, define the function `canvasCodeFunction`, which should loop through all the markers and define what each marker should look like. Here is an example: 

``` javascript
function canvasCodeFunction(ctx){
    for (let marker of markers.value){
        const shape = new Path2D();
        shape.rect(
            imageMarker.value.calculateMarkerXPositionRect(marker.markerLocationX,marker.markerWidth),
            imageMarker.value.calculateMarkerYPositionRect(marker.markerLocationY,marker.markerHeight),
            imageMarker.value.calculateMarkerWidthRect(marker.markerWidth ),
            imageMarker.value.calculateMarkerHeightRect(marker.markerHeight)
        )
        ctx.strokeStyle = "red";
        ctx.fillStyle = "red";
        ctx.fill(shape);
        ctx.stroke(shape);
    }
};
```

This will make all the markers red rectangles of the given width and height.
## Adding Markers
To add markers on click, you can use the `addMarkersOnClick` prop. You then need to define a function in the parent component that you call when a user clicks on the canvas, which I have called `addNewMarker`. This function then calls the `canvasCodeFunction` with the templateCanvas value as an argument.

``` javascript
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

When calling ImageMarker, you also need to make sure that you include `addMarkersOnClick` to be true (the default is false), and also that you include the onClick emit to call your `addNewMarker` function when a new marker needs to be added.

``` javascript
:addMarkersOnClick="true"
@onClick="addNewMarker"
```

## Examples
Here are some example marker designs on a generic image. All the markers are added using the 'click to add Marker' functionality, except for the markers at (0,0) and at (70,70), which I have in place to check that the markers are centered correctly, and also that they are the size and shape that I expect them to be.
### Smiley Faces 
<img width="1052" height="703" alt="smiley_faces" src="https://github.com/user-attachments/assets/e4980301-f87c-492a-9c08-d4181c759bef" />
The canvasCodeFunction function here is:

``` javascript
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
        const mouth = new Path2D();
        mouth.arc(
            imageMarker.value.calculateMarkerXPositionArc(marker.markerLocationX,marker.markerWidth),
            imageMarker.value.calculateMarkerYPositionArc(marker.markerLocationY + 2,marker.markerHeight),
            imageMarker.value.calculateMarkerWidthArc(marker.markerWidth - 6),
            0,
            Math.PI
        );
        ctx.stroke(mouth);
        ctx.fillStyle = "red";
        ctx.fill(mouth);
        shape.rect(
            imageMarker.value.calculateMarkerXPositionRect(marker.markerLocationX,marker.markerWidth),
            imageMarker.value.calculateMarkerYPositionRect(marker.markerLocationY,marker.markerHeight),
            imageMarker.value.calculateMarkerWidthRect(marker.markerWidth),
            imageMarker.value.calculateMarkerHeightRect(marker.markerHeight)
        )
    }
};
```

### Hearts
<img width="1157" height="772" alt="image" src="https://github.com/user-attachments/assets/5a4dac72-e866-481c-b869-32e7c58ce414" />
The canvasCodeFunction function here is:

``` javascript
function canvasCodeFunction(ctx){
    for (let marker of markers.value){
        ctx.strokeStyle = "black";
        ctx.fillStyle = "red";
        let x = imageMarker.value.calculateMarkerXPositionArc(marker.markerLocationX,0);
        let y = imageMarker.value.calculateMarkerYPositionArc(marker.markerLocationY,0);
        ctx.moveTo(x, y);
        ctx.beginPath();
        ctx.bezierCurveTo(x, y-5, x-5, y-15, x-25, y-15);
        ctx.bezierCurveTo(x-55, y-15, x-55, y+15, x-55, y+15);
        ctx.bezierCurveTo(x-55, y+40, x-35, y+50, x, y+80);
        ctx.bezierCurveTo(x+45, y+60, x+55, y+40, x+55, y+15);
        ctx.bezierCurveTo(x+55, y+15, x+55, y-15, x+25, y-15);
        ctx.bezierCurveTo(x+10, y-15, x, y-5, x, y-5);
        ctx.fill();
    }
};
```

### Different Colour and Different Size Rectangles
<img width="1151" height="772" alt="image" src="https://github.com/user-attachments/assets/47de05f5-62e6-432c-9fc0-804b3d9bc914" />
The canvasCodeFunction function here is:

``` javascript
function canvasCodeFunction(ctx){
    let i = 0;
    let size = 0;
    let colours = ["red", "blue", "orange", "green", "yellow", "#03456b", "#02ac70", "pink" ];
    for (let marker of markers.value){
        const shape = new Path2D();
        shape.rect(
            imageMarker.value.calculateMarkerXPositionRect(marker.markerLocationX,marker.markerWidth),
            imageMarker.value.calculateMarkerYPositionRect(marker.markerLocationY,marker.markerHeight),
            imageMarker.value.calculateMarkerWidthRect(marker.markerWidth + size),
            imageMarker.value.calculateMarkerHeightRect(marker.markerHeight + size)
        )
        let fillStyleString = colours[i % colours.length];
        size++;
        i++;
        ctx.fillStyle = fillStyleString;
        ctx.stroke(shape);
        ctx.fill(shape);
    }
};
```

