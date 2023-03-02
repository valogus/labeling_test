<script setup>
import CanvasSelect from 'canvas-select'
import { ref, onMounted, watch } from 'vue';
const canvasRef = ref(null);
const instance = ref(null)
const imageUrl = ref(null);
const dataArea = ref(null);
const labelRef = ref(null)

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
function changeLable(label, index) {
  console.log(labelRef)
  const newData = [...instance.value.dataset].map((el) => {
    if (el.label === label){
      el.label = 'untiteld'
    }
    return el
})
  console.log(newData)
  instance.value.setData(newData)
}

function deleteLable(index) {
  instance.value.deleteByIndex(index)
}

onMounted(() => {

  instance.value = new CanvasSelect(
    canvasRef.value,
    "https://cdn.jsdelivr.net/npm/@heylight/cdn@%5E1/img/onepiece.png"
  );
  instance.value.MIN_HEIGHT = 500
  instance.value.MIN_WIDTH = 1000

  instance.value.on("updated", (result) => {
    const list = [...result];
    list.sort((a, b) => a.index - b.index);
    dataArea.value = list;
  });

  instance.value.on("add", (result) => {
       result.label= 'untiteld'
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
  <input type="file" @change="handleFileUpload" accept="image/*">
  <button @click="change(2)">Добавить полигон</button>
  <div :style="{ width: '1020px', heigth: '550px' }" class="canvas">

  </div>
  <canvas class="container" ref="canvasRef"></canvas>
  <div v-for="(data, index) in dataArea" :key="data.uuid">
    <div @click="changeLable(data.label, index)" ref="labelRef">
      {{ data.label }}
    </div>
    <button @click="deleteLable(index)">
      Удалить
    </button>
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

nav {
  padding: 30px;
}

nav a {
  font-weight: bold;
  color: #2c3e50;
}

nav a.router-link-exact-active {
  color: #42b983;
}

.canvas {
  display: flex;
  align-items: center;
  justify-content: center;

}
</style>
