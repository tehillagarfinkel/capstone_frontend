<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <h1>All Categories</h1>
    <div v-for="category in categories">
      <img v-bind:src="category.image" alt="category.name" />
      <h2>{{ category.name }}</h2>
    </div>
  </div>
</template>

<style></style>

<script>
import axios from "axios";

export default {
  data: function() {
    return {
      message: "Welcome to Vue.js!",
      categories: []
    };
  },
  created: function() {
    axios.get("/api/categories").then(response => {
      console.log(response.data);
      this.categories = response.data;
    });
  },
  methods: {}
};
</script>
