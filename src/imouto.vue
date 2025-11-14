<script setup lang="ts">
import { ref } from 'vue';
const name = 'Imouto';
const status = ref('sleeping');
const games = ref(['imoutoraibu', 'imoutoecchi', 'imoutorabu']);
const newGame = ref('');

const addGame = () => {
  if (newGame.value.trim() !== '') {
    games.value.push(newGame.value);
    newGame.value = '';
  }
};

const deleteGame = (index: number) => {
  games.value.splice(index, 1);
};

const toggleStatus = () => {
  if (status.value === 'sleeping') {
    status.value = 'eating';
  } else if (status.value === 'eating') {
    status.value = 'gaming';
  } else {
    status.value = 'sleeping';
  }
};
</script>

<template>
  <h1>{{ name }}</h1>
  <p v-if="status === 'sleeping'">Sleeping right now!</p>
  <p v-else-if="status === 'eating'">Eating right now!</p>
  <p v-else>Gaming right now!</p>

  <h3>Games:</h3>
  <ul>
    <li v-for="(game, index) in games" :key="game">
      <span>{{ game }}</span>
      <button @click="deleteGame(index)">x</button>
    </li>
  </ul>

  <form @submit.prevent="addGame">
    <label for="newGame">Add a game</label>
    <input type="text" id="newGame" name="newGame" v-model="newGame" />
    <button type="submit">submit</button>
  </form>

  <br />
  <button @click="toggleStatus">Change imouto status</button>
</template>

<style scoped></style>
