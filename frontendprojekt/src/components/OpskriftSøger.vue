<script setup>
import { ref } from 'vue';

const søgeInput = ref('');

const opskriftOutput = ref([]);

const favoritOpskrift = ref([]);

favoritOpskrift.value = JSON.parse(localStorage.getItem('favoritter')) || [];

const findOpskifter = () => { 
    fetch(`https://api.spoonacular.com/recipes/complexSearch?query=${søgeInput.value}&apiKey=d95e6064b25548ff938cc47738f1a41a`)
    .then((response) => response.json())
    .then((data) => { 
        opskriftOutput.value = data.results;
        localStorage.setItem('søgeresultater', JSON.stringify(opskriftOutput.value));
    })
    .catch((error) => console.error(error));
}

const rydSøgningsliste = () => {
    søgeInput.value = '';
    opskriftOutput.value = [];
    localStorage.removeItem('søgeresultater')
}

const tilføjFavorit = (item) => {
  const favoritMedSøgeord = {
    id: item.id,
    title: item.title,
    søgeord: søgeInput.value,
  };

  favoritOpskrift.value.push(favoritMedSøgeord);
  localStorage.setItem('favoritter', JSON.stringify(favoritOpskrift.value));
}



const fravælgFavorit = (item) => {

  const opdateretListe = favoritOpskrift.value.filter(favorit => favorit.id !== item.id);

  favoritOpskrift.value = opdateretListe;

  localStorage.setItem('favoritter', JSON.stringify(favoritOpskrift.value));
}

</script>

<template>
<h2>OpskriftSøger</h2>
<div class="align">
    <form v-on:submit.prevent="findOpskifter">
        <input v-model="søgeInput" placeholder="Find opskrift" id="">
        <button>Søg</button>
    </form>
    <button v-on:click="rydSøgningsliste">Ryd</button>
</div>
<ul>
    <li v-for="item in opskriftOutput" :key="item.id">
        {{ item.title}}
        <button v-on:click="tilføjFavorit(item)">🤍</button>
    </li>
</ul>

<h3>Dine favoritter</h3>
<ul>
    <li v-for="item in favoritOpskrift" :key="item.id">
        <p>Du søgte på <i>{{ item.søgeord }}</i></p>
        <div class="alignfavorit">
            <li>{{ item.title }}</li>
            <button v-on:click="fravælgFavorit(item)">❤️</button>
        </div>
    </li>
</ul>
</template>

<style>
h2{
    font-family: sans-serif;
}

li{
    list-style: "- ";
}

button {
    background-color: red;
    color: white;
    padding: 10px;
    margin: 20px;
}

.align{
    display: flex;
    flex-direction: row;
}

.alignfavorit{
    display: flex;
    align-items: center;
}
</style>