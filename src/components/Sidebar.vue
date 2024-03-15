<template>
  <div class="sidebar">
    <div v-if="loaded">
      <div class="item">available: {{ balance + " " + currency }}</div>
      <div class="item">bonus: {{ bonus }}</div>
      <div class="item">in-play: {{ inPlay }}</div>
    </div>
    <div v-else>
      <div class="item">Loading...</div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      balance: null,
      currency: "",
      bonus: null,
      inPlay: null,
      loaded: false,
    };
  },
  mounted() {
    this.fetchBalance();
    this.interval = setInterval(this.fetchBalance, 30000);
  },
  beforeDestroy() {
    clearInterval(this.interval);
  },
  methods: {
    async fetchBalance() {
      try {
        const token = localStorage.getItem("token");
        const response = await axios.get(
          `https://poker.evenbetpoker.com/api/web/v2/users/me/balance?clientId=default&auth=${token}`
        );
        console.log(response);
        this.currency = response.data.data[0].attributes.currency;
        this.balance = response.data.data[0].attributes.available;
        this.bonus = response.data.data[0].attributes.bonus;
        this.inPlay = response.data.data[0].attributes["in-play"];

        this.loaded = true;
      } catch (error) {
        console.error("Ошибка при получении баланса:", error);
      }
    },
  },
};
</script>
<style>
.sidebar {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
.item {
  font-size: 28px;
  margin-bottom: 30px;
}
</style>
