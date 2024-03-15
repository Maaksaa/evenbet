<template>
  <div class="card">
    <img class="card-img" :src="game.attributes.image" :alt="game.attributes.title" />
    <div class="card-title">{{ game.attributes.title }}</div>
    <button class="card-btn" @click="playDemo">Play Demo</button>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  props: ['game'],
  methods: {
    async playDemo() {
      try {
        const response = await axios.post(`https://poker.evenbetpoker.com/api/web/v2/casino/games/${this.game.id}/session-demo?clientId=default`, {
          clientId: "default",
          gameId: this.game.id,
        });
        console.log(response)
        window.open(response.data.data[0].attributes["launch-options"]["game-url"], '_blank');
      } catch (error) {
        console.error('Ошибка при запуске демо игры:', error);
      }
    }
  }
};
</script>
<style scoped>
.card {
  display: flex;
  flex-direction: column;
  margin: 40px;
  width: 280px;
  border-radius: 20px;
  transition: transform 0.3s;
  background-color: #262626;
  
  /* background-image: linear-gradient(163deg, #00ff75 0%, #3700ff 100%); */
  

}
.card-img {
  width: 100%;
  height: 250px;
  border-radius: 20px 20px 0 0;
  object-fit: cover;
}
.card:hover {
 /* border-color: #008bf8; */
 /* box-shadow: 0 4px 18px 0 rgba(0, 0, 0, 0.25); */
 box-shadow: 0px 0px 30px 1px rgba(0, 255, 117, 0.90);
 transform: scale(1.03);
}
.card-title {
  text-align: center;
  margin-top: 10px;
}
.card-btn {
  
}
button {
  display: block;
  margin: 10px auto ;
  position: relative;
  padding: 10px 20px;
  border-radius: 7px;
  border: 1px solid rgba(0, 255, 117, 0.90);
  font-size: 14px;
  text-transform: uppercase;
  font-weight: 600;
  letter-spacing: 2px;
  background: transparent;

  cursor: pointer;
  color: #e8eaed;
  overflow: hidden;
  box-shadow: 0 0 0 0 transparent;
  -webkit-transition: all 0.2s ease-in;
  -moz-transition: all 0.2s ease-in;
  transition: all 0.2s ease-in;
}

button:hover {
  background: rgba(0, 255, 117, 0.70);
  box-shadow: 0 0 30px 5px rgba(0, 255, 117, 0.90);
  -webkit-transition: all 0.2s ease-out;
  -moz-transition: all 0.2s ease-out;
  transition: all 0.2s ease-out;
}

button:hover::before {
  -webkit-animation: sh02 0.5s 0s linear;
  -moz-animation: sh02 0.5s 0s linear;
  animation: sh02 0.5s 0s linear;
}

button::before {
  content: '';
  display: block;
  width: 0px;
  height: 86%;
  position: absolute;
  top: 7%;
  left: 0%;
  opacity: 0;
  background: #fff;
  box-shadow: 0 0 50px 30px #fff;
  -webkit-transform: skewX(-20deg);
  -moz-transform: skewX(-20deg);
  -ms-transform: skewX(-20deg);
  -o-transform: skewX(-20deg);
  transform: skewX(-20deg);
}

@keyframes sh02 {
  from {
    opacity: 0;
    left: 0%;
  }

  50% {
    opacity: 1;
  }

  to {
    opacity: 0;
    left: 100%;
  }
}

button:active {
  box-shadow: 0 0 0 0 transparent;
  -webkit-transition: box-shadow 0.2s ease-in;
  -moz-transition: box-shadow 0.2s ease-in;
  transition: box-shadow 0.2s ease-in;
}

</style>
