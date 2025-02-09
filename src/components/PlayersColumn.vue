<template>
  <div class="column">
    <h2>👥 Игроки</h2>
    <form @submit.prevent="addPlayer">
      <input v-model="newPlayer.name" placeholder="Имя игрока" required />
      <label>
        <input type="checkbox" v-model="newPlayer.willingToTryNew" />
        Готов пробовать новые игры
      </label>
      <button type="submit">Добавить игрока</button>
    </form>
    <ul>
      <li v-for="player in players" :key="player.id">
        {{ player.name }} ({{ player.willingToTryNew ? 'Готов к новым играм' : 'Не готов к новым играм' }})
        <button @click="deletePlayer(player.id)">🗑️</button>
      </li>
    </ul>
  </div>
</template>

<script>
import playersApi from '@/api/players'; // Импортируем API-клиент

export default {
  data() {
    return {
      newPlayer: {
        name: '',
        willingToTryNew: false,
        ratings: {}, // Рейтинги игрока (пока пустой объект)
      },
      players: [],
    };
  },
  async created() {
    // Загружаем список игроков при создании компонента
    await this.loadPlayers();
  },
  methods: {
    // Загрузить список игроков
    async loadPlayers() {
      try {
        this.players = await playersApi.getPlayers();
      } catch (error) {
        alert('Не удалось загрузить список игроков.');
      }
    },

    // Добавить нового игрока
    async addPlayer() {
      try {
        await playersApi.addPlayer(this.newPlayer);
        
        await this.loadPlayers();

        this.newPlayer = { name: '', willingToTryNew: false, ratings: {} }; // Сбрасываем форму
      } catch (error) {
        alert('Не удалось добавить игрока.');
      }
    },

    // Удалить игрока
    async deletePlayer(id) {
      try {
        await playersApi.deletePlayer(id);
        this.players = this.players.filter((player) => player.id !== id); // Удаляем игрока из списка
      } catch (error) {
        alert('Не удалось удалить игрока.');
      }
    },
  },
};
</script>

<style scoped>
.column {
  border: 1px solid #ddd;
  padding: 20px;
  border-radius: 8px;
  background-color: #f9f9f9;
}

input, button {
  margin: 5px;
  padding: 8px;
  border: 1px solid #ccc;
  border-radius: 4px;
}

button {
  background-color: #42b983;
  color: white;
  cursor: pointer;
}

button:hover {
  opacity: 0.8;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  background-color: #fff;
  padding: 10px;
  margin-bottom: 5px;
  border-radius: 4px;
  border: 1px solid #ddd;
}
</style>

