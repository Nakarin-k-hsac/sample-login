<template>
  <div class="login-container">
    <h1 class="title-text">Sign In</h1>
    <form @submit.prevent="checkUsername">
      <input
          class="email"
          type="text"
          v-model="username"
          placeholder="Email or phone number"
      />
      <div class="empty-box">
        <span class="user-span" v-show="alertEmail">Please enter a valid email or phone number.</span>
      </div>
      <input
          class="password"
          type="password"
          v-model="password"
          placeholder="Password"
      />
      <div class="empty-box">
        <span class="password-span"  v-show="alertPassword">Your password must contain between 4 and 60 characters.</span>
      </div>
      <input class="submit-btn" type="submit" value="Sign In" />
      <div class="remember-me">
        <label><input type="checkbox" v-model="rememberMe" /> Remember me</label>
        <a href="#">Need help?</a>
      </div>
    </form>
    <p class="netflix">New to Netflix? <router-link to="/register">Sign up now</router-link>.</p>
    <p class="captcha">
      This page is protected by Google reCAPTCHA to ensure you're not a bot.
      <a class="learn" href="#">Learn more</a>.
    </p>
  </div>

</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      username: "",
      password: "",
      rememberMe: false,
      alertEmail: false,
      alertPassword: false,
    };
  },
  methods:{
    checkUsername() {
      const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
      const phoneRegex = /^0\d{9}$/;

      if (emailRegex.test(this.username) || phoneRegex.test(this.username)) {
        this.checkPassword();
      } else {
        this.alertEmail = true
      }
    },
    checkPassword(){
      if (this.password.length < 4 || !this.password){
        this.alertPassword = true
      } else {
        this.login()
      }

    },
    async login(){
      const userData = {
        username: this.username,
        password: this.password,
      };

      await axios.post(import.meta.env.VITE_API_URL+"/api/get_users",userData)
          .then((res) => {
            console.log(res.data)
            alert("Success")
            this.$router.push("/home")
          })
          .catch((error) => {
            console.error("There was an error!", error.response.data);
            alert("Error: " + error.response.data);
          });
    }
  },
}
</script>

<style scoped>

.login-container {
  background-color: rgba(0, 0, 0);
  padding: 60px;
  width: 100%;
  max-width: 400px;
  box-shadow: 0 0 15px rgba(0, 0, 0, 0.5);
}

.login-container h1 {
  text-align: start;
  color: white;
  margin-bottom: 20px;
}

.login-container .email,
.login-container .password {
  width: 100%;
  padding: 12px;
  margin: 10px 0 0 0;
  display: inline-block;
  border: none;
  box-sizing: border-box;
  border-radius: 4px;
  background-color: #444444;
  color: white;
  border-bottom: 1px solid orange;
}

.empty-box {
  height: 20px;
}

.user-span {
  font-size: 12px;
  color: orange;
}
.password-span {
  font-size: 12px;
  color: orange;
}

.login-container .submit-btn {
  width: 100%;
  background-color: #e50914;
  color: white;
  padding: 14px 20px;
  margin: 8px 0;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.login-container .submit-btn:hover {
  background-color: #f40612;
}

.remember-me {
  display: flex;
  color: #b3b3b3;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 10px;
  font-size: 12px;
}

.remember-me label {
  display: flex;
  align-items: center;
}

.netflix {
  margin-bottom: 20px;
}

.remember-me input[type="checkbox"] {
  margin-right: 5px;
}

.login-container a {
  color: #b3b3b3;
  text-decoration: none;
}

.login-container a:hover {
  text-decoration: underline;
}

.login-container p {
  color: #656565;
  text-decoration: none;
}

.login-container .learn {
  color: deepskyblue;
  text-decoration: none;
}

.login-container .captcha {
  font-size: 12px;
}


@media (max-width: 600px) {
  .login-container {
    padding: 15px;
    max-width: 100%;
  }

  .login-container input {
    padding: 10px;
  }

  .login-container input[type="submit"] {
    padding: 12px;
  }
}
</style>
