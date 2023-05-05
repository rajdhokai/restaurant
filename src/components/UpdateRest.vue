<template>
  <HeaderPage />
  <h1>Welcome to the Restaurant Page!</h1>
  <h3>You Can update new restaurant here....</h3>

  <form class="add">
    <input
      type="text"
      name="name"
      placeholder="Enter Restaurant name"
      v-model="restaurant.name"
    />
    <input
      type="text"
      name="contact"
      placeholder="Enter Contact number"
      v-model="restaurant.contact"
    />
    <input
      type="text"
      name="address"
      placeholder="Enter address"
      v-model="restaurant.address"
    />
    <button type="submit" v-on:click="updateRestaurant()">
      Update Restaurant
    </button>
  </form>
</template>

<script>
import axios from "axios";
import HeaderPage from "./HeaderPage.vue";
export default {
  name: "UpdateRest",
  components: {
    HeaderPage,
  },
  data() {
    return {
      restaurant: {
        name: "",
        contact: "",
        address: "",
      },
    };
  },
  methods: {
    async updateRestaurant() {
      const result = await axios.put(
        "http://localhost:3000/restaurant/" + this.$route.params.id,
        {
          name: this.restaurant.name,
          contact: this.restaurant.contact,
          address: this.restaurant.address,
        }
      );
      console.log("result", result);
      if (result.status === 200) {
        this.$router.push({ name: "HomePage" });
      }
    },
  },
  async mounted() {
    let user = localStorage.getItem("user-data");
    if (!user) {
      this.$router.push({ name: "login" });
    }
    let result = await axios.get(
      "http://localhost:3000/restaurant/" + this.$route.params.id
    );
    this.restaurant = result.data;
  },
};
</script>

<style></style>
