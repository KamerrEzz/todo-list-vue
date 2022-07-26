<script setup>
import { ref, defineEmits } from 'vue';

const text = ref('');
const emits = defineEmits(["onNewItem"])

function handleSubmit() {
    if (text.value !== '') {
        emits("onNewItem", text);
        text.value = '';
    }
}

</script>

<template>

    <form action="" @submit.prevent="handleSubmit">
        <input type="text" v-model="text" placeholder="Pasear al perro">
    </form>
</template>

<style scoped>

input {
    width: 100%;
    padding: 5px;
    border: 1px solid #2f2f2f;
    border-radius: 4px;
    font-size: 16px;
    background: #1d1d1d;
    outline: none;
    margin: 10px 0;
    color: white;
}

</style>