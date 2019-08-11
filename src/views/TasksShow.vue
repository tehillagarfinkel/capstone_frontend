<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <h1>{{ task.description }}</h1>
    <p>Duration: {{ task.duration }}</p>
    <p>Start Time: {{ task.start_time }}</p>
    <p>Due Date: {{ task.due_date }}</p>
    <p>Completed: {{ task.completed }}</p>
    <router-link v-bind:to="`/category/${task.category_id}`">Back to all tasks</router-link>
  </div>
</template>

<style></style>

<script>
import axios from "axios";
export default {
  data: function() {
    return {
      message: "Welcome to Vue.js!",
      task: {}
    };
  },
  created: function() {
    axios.get("/api/tasks/" + this.$route.params.id).then(response => {
      this.task = response.data;
    });
  },
  methods: {}
};
</script>
