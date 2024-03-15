<template>
  <div>
    <RecycleScroller
      :items="games"
      :item-size="380"
      :buffer="1000"
      :grid-items="4"
      class="scroller"
      key-field="id"
    >
      <template v-slot="{ item }">
        <GameCard :key="item.id" :game="item" />
      </template>
    </RecycleScroller>
  </div>
</template>

<script>
import axios from "axios";
import GameCard from "./GameCard.vue";
import { RecycleScroller } from "vue-virtual-scroller";
import "vue-virtual-scroller/dist/vue-virtual-scroller.css";

export default {
  components: {
    GameCard,
    RecycleScroller,
  },
  data() {
    return {
      games: [],
    };
  },
  mounted() {
    this.fetchGames();
  },
  methods: {
    async fetchGames() {
      try {
        const response = await axios.get(
          "https://poker.evenbetpoker.com/api/web/v2/casino/games?clientId=default"
        );
        this.games = response.data.data;
      } catch (error) {
        console.error("Ошибка при получении списка игр:", error);
      }
    },
  },
};
</script>
<style>
.scroller {
  height: 100%;
}
</style>
