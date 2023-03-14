<script setup>

import Card from "./components/Card.vue" 
import Grid from "./components/Grid.vue"
import RandomMovie from "./components/RandomMovie.vue";
import { ref, reactive } from 'vue'
import Moveable from "vue3-moveable";

const cards = ref([
  { id: 1 },
  { id: 2 },
]);
const grids = ref([
  { id: 1 },
]);
const films = ref([
  { id: 1 },
]);
const cardCount = ref(2);
const gridCount = ref(1);
const filmCount = ref(1);
const targetRef = ref(null);


function addCard() {
  cardCount.value += 1;
  cards.value.push({ id: cardCount.value });
}

function addGrid() {
  gridCount.value += 1;
  grids.value.push({ id: gridCount.value });
}

function addFilm() {
  filmCount.value += 1;
  films.value.push({ id: filmCount.value });
}

function delCard(idx) {
  cards.value.splice(idx, 1);
}

function delGrid(idx) {
  grids.value.splice(idx, 1);
}

function delFilm(idx) {
  films.value.splice(idx, 1);
}

function onDrag(e) {
  e.target.style.transform = e.transform;
}

function selectItem(e) {
  console.log(e.target.className);
  switch (e.target.className) {
    case 'card':
    case 'grid-controls':
    case 'container':
      targetRef.value = e.target.parentNode;
      break;
    case 'poster':
    case 'year':
    case 'rating':
    case 'name':
      targetRef.value = e.target.parentNode.parentNode;
      break;
    default:
      targetRef.value = null;
  }
}

</script>



<template>
  <div class="container"  @click="selectItem">
    <div class="controls">
      <button @click="addCard">Add Card</button>
      <button @click="addGrid">Add Grid</button>
      <button @click="addFilm">Add Film</button>
    </div>
    <Card v-for="(item, i) in cards" 
      class="card-section" 
      :key="item.id" 
      @del="delCard(i)"
      :style="{ margin: Math.floor(Math.sin(item.id) * 50) + 'px' + ' ' + Math.floor(Math.cos(item.id) * 50) + 'px'}"
    />
    <Grid v-for="(item, i) in grids"
      class="grid-section"
      :key="item.id"
      @del="delGrid(i)"
    />
    <RandomMovie v-for="(item, i) in films"
      class="film-section"
      :key="item.id"
      @del="delFilm(i)"
    />
    <Moveable 
      :target = "targetRef"                      
      :draggable = "true"
      :origin="false"

      :snappable="true"
      :bounds="{ left:0, top:0, right:5, bottom:5, position:'css' }"
      @drag = "onDrag"
    /> 
  </div>

</template> 




<style lang="scss">

.controls{
  position: absolute;
  top:20px;
  left: 20px;
}

.container{
  min-height: 90%;
}

.card-section{
  position: absolute;
}

.grid-section{
  position: absolute;
  width: fit-content;
  background-color: lavender;
}

.film-section{
  position: absolute;
}

</style>