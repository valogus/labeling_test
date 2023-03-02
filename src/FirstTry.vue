<script setup>
import { ref, onMounted, watch } from 'vue';
import { fabric } from 'fabric';
import { slotFlagsText } from '@vue/shared';
import  CanvasSelect from 'canvas-select'

const canvasRef = ref(null);
const canvasRef2 = ref(null);
const instance = new CanvasSelect(
  canvasRef.value,
  "https://cdn.jsdelivr.net/npm/@heylight/cdn@%5E1/img/onepiece.png"
);
const option = [
  {
    label: "矩形",
    coor: [
      [184, 183],
      [275, 238]
    ],
    type: 1
  },
  {
    label: "多边形",
    coor: [
      [135, 291],
      [129, 319],
      [146, 346],
      [174, 365],
      [214, 362],
      [196, 337],
      [161, 288]
    ],
    type: 2
  },
  {
    label: "点",
    coor: [345, 406],
    type: 3
  },
  {
    label: "折线",
    coor: [
      [470, 155],
      [503, 230],
      [506, 298]
    ],
    type: 4
  },
  {
    label: "圆形",
    coor: [369, 197],
    radius: 38,
    type: 5
  }
];
console.log(instance)
instance.setData(option);
    const imageUrl = ref(null);
    let objects = [];
    const handleFileUpload = (event) => {
      const file = event.target.files[0];
      const reader = new FileReader();

      reader.onload = () => {
        imageUrl.value = reader.result;
      };

      reader.readAsDataURL(file);
    };
   

//     watch(imageUrl, (newImageUrl) => {
//         // create canvas layer
//       if (newImageUrl) {
//         const canvas = new fabric.Canvas(canvasRef.value, {
//             backgroundColor:'grey'
// });
//         canvas.setHeight(600);
//         canvas.setWidth(1000);
//         //add new image
//        fabric.Image.fromURL(newImageUrl, (img) => {
//         canvas.add(img);
        
//           canvas.setActiveObject(img);
//           const mainImage= canvas.getActiveObject();
//           mainImage.hasControls = false;
//           canvas.renderAll();
//           imageUrl.value = null
//         });
//         var rect = new fabric.Rect({
//   left: 100,
//   top: 100,
//   fill: 'red',
//   width: 20,
//   height: 20
// });
// canvas.add(rect);
//         // add zoom
//         canvas.on('mouse:wheel', (event) => {
//           const delta = event.e.deltaY;
//           let zoom = canvas.getZoom();
//           zoom = zoom + delta / 200;
//           if (zoom > 20) zoom = 20;
//           if (zoom < 0.01) zoom = 0.01;
//           canvas.zoomToPoint({ x: event.e.offsetX, y: event.e.offsetY }, zoom);
//           event.e.preventDefault();
//           event.e.stopPropagation();
//         });
        // add motion
        // canvas.on('mouse:down', (event) => {
        //   canvas.isDragging = true;
        //   canvas.selection = false;
        //   canvas.lastPosX = event.e.clientX;
        //   canvas.lastPosY = event.e.clientY;
        // });
  
        
    
    // const addLabel = (e) => {
        
    // //   const { offsetX, offsetY } = e;
    //   const offsetX = e.e.clientX
    //   const offsetY = e.e.clientY
    //   const points = [
    //     { x: offsetX - 50, y: offsetY - 50 },
    //     { x: offsetX + 50, y: offsetY - 50 },
    //     { x: offsetX + 50, y: offsetY + 50 },
    //     { x: offsetX - 50, y: offsetY + 50 },
    //   ];
    //   console.log(e)
    //   const polygon = new fabric.Polygon(points, {
    //     fill: 'rgba(0, 0, 0, 0)',
    //     stroke: 'red',
    //     strokeWidth: 2,
    //   });
    //   canvas.add(offsetX, offsetY );

    //   const label = new fabric.Textbox('Label', {
    //     left: offsetX,
    //     top: offsetY,
    //     width: 100,
    //     fontSize: 16,
    //     textAlign: 'center',
    //     borderColor: 'red',
    //     cornerColor: 'red',
    //     cornerSize: 8,
    //   });
    //   canvas.add(label);
    //   objects.push(polygon, label);
    // };
    // canvas.on('mouse:down', addLabel);
        // canvas.on('mouse:move', (event) => {
        //   if (canvas.isDragging) {
        //     const e = event.e;
        //     const zoom = canvas.getZoom();
        //     const deltaLeft = e.clientX - canvas.lastPosX;
        //     const deltaTop = e.clientY - canvas.lastPosY;
        //     const deltaLeftScaled = deltaLeft / zoom;
        //     const deltaTopScaled = deltaTop / zoom;
        //     canvas.viewportTransform[4] += deltaLeftScaled;
        //     canvas.viewportTransform[5] += deltaTopScaled;
        //     canvas.requestRenderAll();
        //     canvas.lastPosX = e.clientX;
        //     canvas.lastPosY = e.clientY;
        //   }
        // });
        // //stop motion
        // canvas.on('mouse:up', () => {
        //   canvas.isDragging = false;
        //   canvas.selection = true;
        // });
    //   }
    // });
</script>

<template>
    <input type="file" @change="handleFileUpload" accept="image/*">
    <div class="canvas">
    <!-- <canvas class="canvas" ref="canvasRef"></canvas> -->
    </div>
    <canvas :style="{ width:520, heigth:480  }" class="canvas2" ref="canvasRef2"></canvas>
</template>
    
<style>
#app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
}

.canvas{
    display: flex;
    align-items: center;
    justify-content: center;
    width:100%;
    height:100%;
}
</style>
  