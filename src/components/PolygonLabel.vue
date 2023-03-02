<script setup>
import { ref } from 'vue';
import styledButton from '@/components/UI/Button.vue'

const props = defineProps({
    data: Object,
})

const emit = defineEmits(['cahngeLabel', 'deleteLabel'])
const isOpen = ref(false)

function handleChangeLabel() {
    if (!isOpen.value) isOpen.value = true
}

function submit(e) {
    const changedTitle = e.target.inputText.value
    emit("cahngeLabel", changedTitle, props.data.uuid)
    isOpen.value = false
}

function handleDeleteLabel() {
    emit("deleteLabel", props.data.uuid)
}

</script>


<template>
        <div @click="handleChangeLabel">
            <div v-if="data.label && !isOpen">{{ data.label }}</div>
            <form v-else-if="isOpen" @submit.prevent="submit">
                <input name="inputText" :value="data.label">
            </form>
            <div v-else>untiteld</div>

        </div>
        <styledButton @click.stop="handleDeleteLabel">
            Удалить
        </styledButton>
</template>