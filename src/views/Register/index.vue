<template>
  <div class="login-container">
    <h1 class="title-text">Sign Up</h1>
    <form @submit.prevent="checkUsername">
      <input
          class="text_box"
          type="text"
          v-model="firstName"
          placeholder="Firstname"
          required
      />
      <input
          class="text_box"
          type="text"
          v-model="lastName"
          placeholder="LastName"
          required
      />
      <input
          class="text_box"
          type="text"
          v-model="username"
          placeholder="Username (email or phone number)"
          required
      />
      <input
          class="password"
          type="password"
          v-model="password"
          placeholder="Password"
          required
      />
      <input
          class="password"
          type="password"
          v-model="secondPassword"
          placeholder="Re-Password"
          required
      />
      <input class="submit-btn" type="submit" value="Sign Up" />

    </form>
    <p class="captcha">
      This page is protected by Google reCAPTCHA to ensure you're not a bot.
      <a class="learn" href="#">Learn more</a>.
    </p>
  </div>

</template>

<script>
import axios from "axios";
import CryptoJS from "crypto-js";

export default {
  data() {
    return {
      username: "",
      password: "",
      secondPassword: "",
      firstName: "",
      lastName: "",
    };
  },
  methods:{
    checkUsername() {
      const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
      const phoneRegex = /^0\d{9}$/;

      if (emailRegex.test(this.username) || phoneRegex.test(this.username)) {
        this.checkPassword();
      } else {
        alert("Username must be an email or a phone number.");
      }
    },
    checkPassword(){
      if (this.password.length < 4){
        alert("Your password must contain between 4 and 60 characters")
      } else {
        if (this.password === this.secondPassword){
          this.register()
        } else {
          alert("Password don't match")
        }
      }

    },
    async register(){
      const userData = {
        firstName: this.firstName,
        lastName: this.lastName,
        username: this.username,
        password: this.password,
      };

      await axios.post(import.meta.env.VITE_API_URL+"/api/create_users",userData)
          .then((res) => {
            console.log(res.data)
            alert("Success")
            this.$router.push("/")
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

.login-container .text_box,
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
  border-bottom: 1px solid #6eb0fa;
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
  background-color: rgb(59 130 246);
  color: white;
  padding: 14px 20px;
  margin: 8px 0;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.login-container .submit-btn:hover {
  background-color: rgb(32, 89, 181);
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
