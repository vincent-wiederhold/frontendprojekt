<script setup>
import { ref } from 'vue';

const søgeInput = ref('');

const opskriftOutput = ref([]);

const favoritOpskrift = ref([]);

favoritOpskrift.value = JSON.parse(localStorage.getItem('farvoritter')) || [];

const findOpskifter = () => { 
    fetch(`https://api.spoonacular.com/recipes/complexSearch?query=${søgeInput.value}&apiKey=d95e6064b25548ff938cc47738f1a41a`)
    .then((response) => response.jason())
    .then((data))
}

</script>

<template>

</template>

<style>

</style>