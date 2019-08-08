<template>
  <div class="home">
    <h1>{{ category.name }}</h1>
    <img v-bind:src="category.image" alt="" />
    <div v-for="task in categoryTasks">
      <h2>{{ task.description }}</h2>
    </div>
    <router-link to="/">Back to my categories</router-link>
  </div>
</template>

<style></style>

<script>
import axios from "axios";

export default {
  data: function() {
    return {
      message: "Welcome to Vue.js!",
      category: [],
      categoryTasks: []
    };
  },
  created: function() {
    axios.get("/api/categories/" + this.$route.params.id).then(response => {
      console.log(response.data);
      this.category = response.data;
      this.categoryTasks = response.data.tasks;
    });
  },
  methods: {}
};
</script>
