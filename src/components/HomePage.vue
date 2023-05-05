<template>
  <HeaderPage />
  <h1>Hello!😀😀 {{ name }}....</h1>
  <table border="2">
    <tr>
      <td>ID</td>
      <td>Name</td>
      <td>Contact</td>
      <td>Address</td>
      <td>Action</td>
    </tr>
    <tr v-for="item in restaurant" :key="item.id">
      <td>{{ item.id }}</td>
      <td>{{ item.name }}</td>
      <td>{{ item.contact }}</td>
      <td>{{ item.address }}</td>
      <td><router-link :to="'/update-rest/' + item.id">Update</router-link></td>
      <td><button v-on:click="deleterestaurant(item.id)">Delete</button></td>
    </tr>
  </table>
</template>

<script>
import HeaderPage from "./HeaderPage.vue";
import axios from "axios";
export default {
  name: "HomePage",
  data() {
    return { name: "", restaurant: [] };
  },
  components: {
    HeaderPage,
  },
  methods: {
    async deleterestaurant(id) {
      let result = await axios.delete("http://localhost:3000/restaurant/" + id);
      this.restaurant = result.data;
      if (result.status === 200) {
        this.loadData();
      }
    },
    async loadData() {
      let user = localStorage.getItem("user-data");
      this.name = JSON.parse(user).name;
      if (!user) {
        this.$router.push({ name: "SignUp" });
      }
      let result = await axios.get("http://localhost:3000/restaurant");
      this.restaurant = result.data;
    },
  },
  async mounted() {
    this.loadData();
  },
};
</script>

<style>
.td {
  width: 160px;
  height: 40px;
}
</style>
