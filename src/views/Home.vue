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
      <button v-on:click="showCategoryTasks(category)">Show Tasks</button>
      <div v-if="currentCategory === category">
        <div v-for="task in category.tasks">
          {{ task.description }}
        </div>
        <div>
          Name:
          <input v-model="category.name" type="text" />
          Image:
          <input v-model="category.image" type="text" />
          <button v-on:click="updateCategory(category)">Update</button>
        </div>
        <button v-on:click="destroyCategory(category)">Delete</button>
      </div>
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
    },
    showCategoryTasks: function(category) {
      if (this.currentCategory === category) {
        this.currentCategory = {};
      } else {
        this.currentCategory = category;
      }
    },
    updateCategory: function(category) {
      var params = {
        name: category.name,
        image: category.image
      };
      axios.patch("/api/categories/" + category.id, params).then(response => {
        console.log(response.data);
        this.currentCategory = {};
      });
    },
    destroyCategory: function(category) {
      axios.delete("/api/categories/" + category.id).then(response => {
        var index = this.categories.indexOf(category);
        this.categories.splice(index, 1);
      });
    }
  }
};
</script>
