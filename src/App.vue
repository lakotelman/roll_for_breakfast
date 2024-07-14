<script setup lang="ts">
import { RouterLink, RouterView } from "vue-router";
import { computed, ref } from "vue";
import type { Ref, VueElement } from "vue";
import rollforbreakfast from '../src/assets/tables/rollforbreakfast.json'

let roll: Ref<keyof typeof rollforbreakfast | null> = ref(null);

function generateRandom(min = 1, max = 100) {
  let difference = max - min;
  let numRoll = 0
  numRoll = Math.random();
  numRoll = Math.floor(numRoll * difference);
  numRoll = numRoll + min;
  roll.value = numRoll.toString() as keyof typeof rollforbreakfast
}

let response = computed(
  () => {
     if( roll.value === null){ 
    return null
  }
  else{ 
    return rollforbreakfast[roll.value]
  } 
  }
)
</script>

<template>
  <header>
    <div>
      <h1>Roll For Breakfast</h1>
      <button @click="generateRandom()">Roll</button>
      <h2> You are having {{ response?.Description }}</h2>
      <h2> {{ response?.Effect }}</h2>
    </div>
  </header>
</template>

<style scoped></style>
