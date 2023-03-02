<script setup>
import CanvasSelect from 'canvas-select'
import { ref, onMounted, watch } from 'vue';
import Label from '@/components/PolygonLabel.vue'
import styledButton from '@/components/UI/Button.vue'

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
function changeDrawType(num) {
  instance.value.createType = num;
}
function handleChangeLabel(label, id) {
  const newData = instance.value.dataset.map((el) => {
    if (el.uuid === id) {
      el.label = label
    }
    return el
  })
  instance.value.setData(newData)
}

function handleDeleteLabel(id) {
  const newData = instance.value.dataset.filter((el) => el.uuid !== id)
  instance.value.setData(newData)
}
//stop active image
function stopCanvas() {
  const newData = instance.value.dataset.map((el) => {
    el.creating = false
    el.active = false
    return el
  })
  instance.value.setData(newData)
}

onMounted(() => {
  instance.value = new CanvasSelect(canvasRef.value);
//canvas change listener
  instance.value.on("updated", (result) => {
    const list = [...result];
    list.sort((a, b) => a.index - b.index);
    dataArea.value = list;
  });

  instance.value.on("add", (result) => {
    result.label = 'untiteld'
  });

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
  <styledButton @click="changeDrawType(2)">Добавить полигон</styledButton>
  <div class="container">
    <div class="labelList" @click="stopCanvas">
      <div v-for="data in dataArea" :key="data.uuid">
        <Label :data="data" @cahngeLabel="handleChangeLabel" @deleteLabel="handleDeleteLabel"></Label>
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

.canvas {
  position: relative
}


.labelList {
  width: 200px;
}
</style>
