<script setup lang="ts">
import { RouterLink, RouterView } from "vue-router";
import { computed, ref } from "vue";
import type { Ref, VueElement } from "vue";
import rollforbreakfast from "../src/assets/tables/rollforbreakfast.json";

let roll: Ref<keyof typeof rollforbreakfast | null> = ref(null);

function generateRandom(min = 1, max = 100) {
  let difference = max - min;
  let numRoll = 0;
  numRoll = Math.random();
  numRoll = Math.floor(numRoll * difference);
  numRoll = numRoll + min;
  roll.value = numRoll.toString() as keyof typeof rollforbreakfast;
}

let response = computed(() => {
  if (roll.value === null) {
    return null;
  } else {
    return rollforbreakfast[roll.value];
  }
});
</script>

<template>
  <div class="dice-wrapper wavy">
    <div id="dice-pattern"></div>
    <div id="dice-gradient-overlay"></div>
  </div>
  <header class="header-content">
    <h1>Roll For Breakfast</h1>
  </header>
  <section class="prose">
    <button @click="generateRandom()" class="hover-effect">Let's Roll</button>
    <div v-if="response === null && roll === null">
      <h2 style="text-align: center">What will it be??</h2>
    </div>
    <div class="" v-if="response !== null">
      <h2 v-if="roll !== null" style="text-align: center">{{ roll }}/100</h2>
      <p class="label">You are having:</p>
      <p>{{ response?.Description }}</p>
      <p class="label">The effect is:</p>
      <p>{{ response?.Effect }}</p>
    </div>
  </section>
</template>

<style scoped>
@keyframes pan {
  0% {
    background-position: 0% 0%;
  }
  100% {
    background-position: 100% 0%;
  }
}
.prose {
  position: relative;
  top: -30px;
}
button {
  margin: 0px auto;
  margin-bottom: 30px;
}

.label {
  font-weight: 900;
  margin-bottom: 5px;
}

#dice-pattern {
  background-image: url("../src/assets/repeat-d20.svg");
  background-size: 10%;
  position: absolute;
  left: 50%;
  top: 0px;
  translate: -50% 0%;
  z-index: 1;
  height: 100%;
  width: 100%;
  min-width: 1200px;
  animation: pan 80s linear infinite forwards;
  will-change: background-position;
}
#dice-gradient-overlay {
  background: radial-gradient(circle, transparent 75%, var(--rfb-fuchsia-dark));
  position: absolute;
  left: 0px;
  top: 0px;
  width: 100%;
  height: 100%;
  opacity: 0.9;
  z-index: 2;
}
.dice-wrapper {
  background: radial-gradient(
    circle,
    var(--rfb-fuchsia),
    var(--rfb-fuchsia-dark)
  );
  position: relative;
  height: 250px;
  overflow: hidden;
  isolation: isolate;
}

.header-content {
  position: relative;
  z-index: 3;
  background-color: #fff;
  border: 2px solid #000;
  height: max-content;
  width: max-content;
  padding: 1rem 1.5rem;
  border-radius: 30px;
  top: -80px;
  margin: 0px auto;
  box-shadow:
    0px 0px 0px 10px #e34659,
    0px 0px 0px 20px #e87080,
    15px 15px 0px 15px #e9b021,
    -15px -15px 0px 15px #138e97;
}

.wavy {
  --mask: radial-gradient(
        80.62px at 50% calc(100% - 110px),
        #000 99%,
        #0000 101%
      )
      calc(50% - 80px) 0/160px 100%,
    radial-gradient(80.62px at 50% calc(100% + 70px), #0000 99%, #000000 101%)
      50% calc(100% - 40px) / 160px 100% repeat-x;
  mask: var(--mask);
  -webkit-mask: var(--mask);
}
</style>
