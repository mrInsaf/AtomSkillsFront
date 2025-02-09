<template>
  <div class="column">
    <h2>🏆 Результаты</h2>
    <button @click="pickGame">Подобрать игру</button>
    <div v-if="bestGame">
      <h3>Лучшая игра: {{ bestGame.name }}</h3>
      <p>Подходит для {{ bestGame.minPlayers }}-{{ bestGame.maxPlayers }} игроков.</p>
    </div>
    <div v-else>
      <p>Невозможно выбрать игру 😢</p>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    games: {
      type: Array,
      required: true,
    },
    players: {
      type: Array,
      required: true,
    },
  },
  data() {
    return {
      bestGame: null,
    };
  },
  methods: {
    pickGame() {
      // Пример простого алгоритма: выбираем первую игру, которая подходит по количеству игроков
      const numPlayers = this.players.length;
      this.bestGame = this.games.find(
        (game) => game.minPlayers <= numPlayers && game.maxPlayers >= numPlayers
      );
    },
  },
};
</script>

