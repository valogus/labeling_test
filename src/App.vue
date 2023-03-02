<script setup>
import CanvasSelect from 'canvas-select'
import { ref, onMounted, watch } from 'vue';
import Label from '@/components/PolygonLabel.vue'

const canvasRef = ref(null);
const instance = ref(null)
const imageUrl = ref(null);
const dataArea = ref(null);

const handleFileUpload = (event) => {
  const file = event.target.files[0];
  const reader = new FileReader();

  reader.onload = () => {
    imageUrl.value = reader.result;
  };

  reader.readAsDataURL(file);
};

function change(num) {
  instance.value.createType = num;
  console.log(instance.value)
}
function changeLabel(label, id) {
   console.log(label, id)
  const newData = instance.value.dataset.map((el) => {
    if (el.uuid === id) {
      el.label = label
    }
    return el
  })
  console.log(newData)
  instance.value.setData(newData)
}

function deleteLabel(id) {
  const newData = [...instance.value.dataset].filter((el) => el.uuid !== id)
  console.log(newData)
  instance.value.setData(newData)

  // instance.value.deleteByIndex(index)
}

onMounted(() => {
  instance.value = new CanvasSelect(
    canvasRef.value,
    "https://cdn.jsdelivr.net/npm/@heylight/cdn@%5E1/img/onepiece.png",
  );
  instance.value.on("updated", (result) => {
    const list = [...result];
    list.sort((a, b) => a.index - b.index);
    dataArea.value = list;
  });

  instance.value.on("add", (result) => {
    result.label = 'untiteld'
    change(0)
    console.log(instance.value)
  });

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
  // console.log(instance.value.update)
  instance.value.setData(option);
})

watch(imageUrl, (newImageUrl) => {
  if (newImageUrl) {
    instance.value.setImage(newImageUrl)
    instance.value.setData([])
  }
});
</script>




<template>
  <input id="file-upload" class="hidden-input" type="file" @change="handleFileUpload" accept="image/*">
  <label for="file-upload" class="btn">
    Загрузить картинку
</label>
  <button class="btn" @click="change(2)">Добавить полигон</button>
  <div class="container">
    <div class="labelList">
    <div v-for="(data, index) in dataArea" :key="data.uuid">
      <Label :data="data" @cahngeLabel="changeLabel" @deleteLabel="deleteLabel"></Label>
    </div>

  </div>
    <canvas width="820" height="462" style="background-color:#ccc" class="canvas" ref="canvasRef"></canvas>

  </div>
</template>





<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}


.container {
  display: flex;
  justify-content: space-evenly;

}
input[type="file"] {
    display: none;
}
.canvas{
  position:relative
}
.btn {
	display: inline-block;	
	box-sizing: border-box;
	padding: 0 25px;
	margin: 0 15px 15px 0;
	outline: none;
	border: 1px solid #fff;
	border-radius: 50px;
	height: 46px;
	line-height: 46px;
	font-size: 14px;
	font-weight: 600;
	text-decoration: none;
	color: #444;
	background-color: #fff;
	box-shadow: 0 4px 6px rgb(65 132 144 / 10%), 0 1px 3px rgb(0 0 0 / 8%);
	cursor: pointer;
	user-select: none;
	appearance: none;
	touch-action: manipulation;  
	vertical-align: top;
	transition: box-shadow 0.2s;
}
.btn:focus-visible {
	border: 1px solid #4c51f9;
	outline: none;
}
.btn:hover {
	transition: all 0.2s;
	box-shadow: 0 7px 14px rgb(65 132 144 / 10%), 0 3px 6px rgb(0 0 0 / 8%);
}
.btn:active {
	background-color: #808080;
}
.btn:disabled {
	background-color: #eee;
	border-color: #eee;
	color: #444;
	cursor: not-allowed;
}
.labelList{
  width:200px;
}
</style>
