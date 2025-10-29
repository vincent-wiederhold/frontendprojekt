<script setup>
import { ref } from 'vue';

const søgeInput = ref('');

const opskriftOutput = ref([]);

const favoritOpskrift = ref([]);

favoritOpskrift.value = JSON.parse(localStorage.getItem('farvoritter')) || [];

const findOpskifter = () => { 
    fetch(`https://api.spoonacular.com/recipes/complexSearch?query=${søgeInput.value}&apiKey=d95e6064b25548ff938cc47738f1a41a`)
    .then((response) => response.jason())
    .then((data) => { 
        opskriftOutput.value = data.results;
        localStorage.setItem('søgeresultater', JSON.stringify(opskriftOutput.value));
    })
    .catch((error) = console.error(error));
}

const rydSøgningsliste = () => {
    søgeInput.value = '';
    opskriftOutput.value = [];
    localStorage.removeItem('søgeresultater')
}
</script>
<h2>Søg på opskrifter</h2>

<template>

</template>

<style>

</style>