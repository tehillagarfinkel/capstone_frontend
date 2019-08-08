<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <h1>New Category</h1>
    Name:
    <input v-model="categoryName" type="text" />
    Image:
    <input v-model="categoryImage" type="text" />
    <button v-on:click="createCategory()">Create Category</button>
    <h1>All Categories</h1>
    <div v-for="category in categories">
      <img v-bind:src="category.image" alt="category.name" />
      <h2>{{ category.name }}</h2>
      <router-link v-bind:to="`/category/${category.id}`">Show Tasks</router-link>
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
      categories: [],
      currentCategory: {},
      categoryName: "",
      categoryImage: ""
    };
  },
  created: function() {
    axios.get("/api/categories").then(response => {
      console.log(response.data);
      this.categories = response.data;
    });
  },
  methods: {
    createCategory: function() {
      var params = {
        name: this.categoryName,
        image: this.categoryImage
      };
      axios.post("/api/categories", params).then(response => {
        console.log(response.data);
        this.categories.push(response.data);
        this.categoryName = "";
        this.categoryImage = "";
      });
    }
  }
};
</script>
