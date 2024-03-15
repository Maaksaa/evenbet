<template>
  <div class="login-box">
    <form @submit.prevent="login">
      <div class="user-box">
        <input
          v-model="credentials.login"
          @focus="inputFocused.login = true"
          @blur="inputFocused.login = false"
          type="text"
        />
        <label
          :class="{ 'active-label': inputFocused.login || credentials.login }"
          >Username</label
        >
      </div>
      <div class="user-box">
        <input
          v-model="credentials.password"
          :type="showPassword ? 'text' : 'password'"
          @focus="inputFocused.password = true"
          @blur="inputFocused.password = false"
        />
        <label
          :class="{
            'active-label': inputFocused.password || credentials.password,
          }"
          >Password</label
        >
        <span class="toggle-password" @click="showPassword = !showPassword">
          <svg id="open" width="24" height="24" v-if="showPassword">
            <g stroke="#fff" stroke-miterlimit="10">
              <path
                d="M21.632 12.5a9.759 9.759 0 01-18.264 0 9.759 9.759 0 0118.264 0z"
                fill="none"
              />
              <circle cx="12.5" cy="12.5" r="3" fill="#fff" />
              <path
                fill="none"
                d="M12.5 5v1-4M9.291 6.337L7.709 2.663M15.709 6.337l1.582-3.674"
              />
            </g>
          </svg>
          <svg id="close" width="24" height="24" v-else>
            <g fill="none" stroke="#fff" stroke-miterlimit="10">
              <path
                d="M21.632 12.5a9.759 9.759 0 01-18.264 0M12.5 19.5v-1 4M9.291 18.163l-1.582 3.674M15.709 18.163l1.582 3.674"
              />
            </g>
          </svg>
        </span>
      </div>
      <p v-if="errorMessage" class="error-message">{{ errorMessage }}</p>
      <button type="submit">Отправить<span></span></button>
    </form>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      showPassword: false,
      errorMessage: "",
      inputFocused: {
        login: false,
        password: false,
      },

      credentials: {
        // login: "richard",
        // password: "poker",
        login: "",
        password: "",
      },
    };
  },
  methods: {
    async login() {
      try {
        let response = await axios.post(
          "https://poker.evenbetpoker.com/api/web/v2/login?clientId=default",
          {
            clientId: "default",
            login: this.credentials.login,
            password: this.credentials.password,
          }
        );
        console.log(response);
        console.log("токен: " + response.data.data[0].attributes.token);
        console.log(
          "рефреш токен: " + response.data.data[0].attributes["refresh-token"]
        );

        let token = response.data.data[0].attributes.token;
        let refreshToken = response.data.data[0].attributes["refresh-token"];
        if (token && refreshToken) {
          localStorage.setItem("token", token);
          localStorage.setItem("refreshToken", refreshToken);
          this.$router.push("/"); // переход на главную
        }
      } catch (error) {
        console.error(error);
        if (error.response && error.response.status === 422) {
          this.errorMessage = "Неверный логин или пароль.";
        } else {
          this.errorMessage = "Произошла ошибка. Пожалуйста, попробуйте снова.";
        }
      }
    },
  },
};
</script>
<style scoped>
.error-message {
  color: red;
  text-align: center;
}
.toggle-password {
  cursor: pointer;
  position: absolute;
  right: 10px;
  top: 50%;
  transform: translateY(-100%);
}
.login-box {
  position: absolute;
  top: 50%;
  left: 50%;
  width: 400px;
  padding: 40px;
  transform: translate(-50%, -50%);
  background: #181414;
  box-sizing: border-box;
  box-shadow: 0 15px 25px rgba(0, 0, 0, 0.6);
  border-radius: 10px;
}

.login-box .user-box {
  position: relative;
}

.login-box .user-box input {
  width: 100%;
  padding: 10px 0;
  font-size: 16px;
  color: #fff;
  margin-bottom: 30px;
  border: none;
  border-bottom: 1px solid #fff;
  outline: none;
  background: transparent;
}

.login-box .user-box label {
  position: absolute;
  top: 0;
  left: 0;
  padding: 10px 0;
  font-size: 16px;
  color: #fff;
  pointer-events: none;
  transition: 0.5s;
}

.login-box .user-box .active-label {
  top: -20px;
  left: 0;
  color: #bdb8b8;
  font-size: 12px;
}

.login-box form button {
  position: relative;
  display: block;
  margin: 0px auto;
  margin-top: 40px;
  padding: 10px 20px;
  color: #ffffff;
  /* background: rgba(36, 29, 29, 0.987); */
  background: #181414;

  cursor: pointer;

  font-size: 16px;
  text-decoration: none;
  text-transform: uppercase;
  overflow: hidden;
  transition: 0.5s;
  letter-spacing: 4px;
  border: 0;
}

.login-box button:hover {
  background: #03f40f;
  color: #fff;
  border-radius: 5px;
  box-shadow: 0 0 5px #03f40f, 0 0 25px #03f40f, 0 0 50px #03f40f,
    0 0 100px #03f40f;
}

.login-box button span {
  position: absolute;
  display: block;
}

@keyframes btn-anim1 {
  0% {
    left: -100%;
  }

  50%,
  100% {
    left: 100%;
  }
}

.login-box button span:nth-child(1) {
  bottom: 2px;
  left: -100%;
  width: 100%;
  height: 2px;
  background: linear-gradient(90deg, transparent, #03f40f);
  animation: btn-anim1 2s linear infinite;
}
</style>
