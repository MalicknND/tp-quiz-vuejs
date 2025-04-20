<template>
  <div class="container">
    <div v-if="state === 'error'">
      <h1>Erreur de chargement</h1>
      <p>Impossible de charger le quiz.</p>
    </div>
    <div :aria-busy="state === 'loading'">
      <Quiz :quiz="quiz" v-if="quiz" />
    </div>
  </div>
</template>

<script setup>
import { onMounted, ref } from "vue";
import Quiz from "./components/Quiz.vue";

const quiz = ref(null);
const state = ref("loading");

onMounted(() => {
  fetch("/quiz.json")
    .then((r) => {
      if (r.ok) {
        return r.json();
      }
      throw new Error("impossible de récuperer le json");
    })
    .then((data) => {
      quiz.value = data;
      state.value = "loaded";
    })
    .catch((e) => {
      console.error(e);
      state.value = "error";
    });
});
</script>

<style>
.container {
  margin-top: 2rem;
}
</style>
