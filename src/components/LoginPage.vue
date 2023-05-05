<template>
  <img class="logo" src="../assets/logo1.png" alt="" />
  <h1>Login</h1>
  <div class="register">
    <input
      type="text"
      v-model="email"
      placeholder="Enter your email here...."
    />
    <input
      type="password"
      v-model="password"
      placeholder="Enter password here...."
    />
    <button type="submit" v-on:click="login">Login</button>
    <p><router-link to="/sign-up">Sign Up</router-link></p>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "LoginPage",
  data() {
    return {
      email: "",
      password: "",
    };
  },
  methods: {
    async login() {
      let result = await axios.get(
        `http://localhost:3000/users?email=${this.email}&password=${this.password}`
      );
      if (result.status === 200 && result.data.length > 0) {
        localStorage.setItem("user-data", JSON.stringify(result.data[0]));
        this.$router.push({ name: "HomePage" });
      }
      console.log("result", result);
    },
  },
};
</script>

<style></style>
