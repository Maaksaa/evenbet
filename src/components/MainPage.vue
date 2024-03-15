<template>
  <div class="container">
    <Sidebar class="sidebar"/>
    <GameList class="game-list"/>
  </div>
</template>

<script>
import axios from 'axios';
import Sidebar from './Sidebar.vue';
import GameList from './GameList.vue';

export default {
  components: {
    Sidebar,
    GameList
  },
  mounted() {
    this.startTokenRefreshInterval();
  },
  methods: {
    startTokenRefreshInterval() {
      setInterval(async () => {
        let refreshToken = localStorage.getItem("refreshToken");
        try {
          let response = await axios.post("https://poker.evenbetpoker.com/api/web/auth/token?clientId=default", {
            clientId: "default",
            refreshToken: refreshToken,
          });
          console.log("токен обновился");
          console.log("новый токен: " + response.data.token);
          console.log("новый рефреш токен: " + response.data["refresh-token"]);

          let newToken = response.data.token;
          let newRefreshToken = response.data["refresh-token"];

          if (newToken && refreshToken) {
            localStorage.setItem("token", newToken);
            localStorage.setItem("refreshToken", newRefreshToken);
          }

          
        } catch (error) {
          console.error('Ошибка при обновлении токена:', error);
        }
      }, 800 * 1000);
    }
  }
};
</script>
<style scoped>

.container {
  display: flex;
  height: 100vh;
}

.sidebar {
  width: 20%; 
}

.game-list {
  flex: 1; 
}
</style>