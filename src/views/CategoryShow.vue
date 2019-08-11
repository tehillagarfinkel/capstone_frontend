<template>
  <div class="home">
    <h1>{{ category.name }}</h1>
    <img v-bind:src="category.image" alt="" />
    <div v-for="task in tasks">
      <h2>{{ task.description }}</h2>
      <router-link v-bind:to="`/tasks/${task.id}`">More details</router-link>
    </div>
    <div>
      <h2>New Task</h2>
      Description:
      <input v-model="taskDescription" type="text" />
      <button v-on:click="createTask">Create Task</button>
    </div>
    <div>
      Name:
      <input v-model="category.name" type="text" />
      Image:
      <input v-model="category.image" type="text" />
      <button v-on:click="updateCategory(category)">Update Category</button>
      <button v-on:click="destroyCategory(category)">Delete Category</button>
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
      tasks: [],
      name: "",
      image: "",
      taskDescription: ""
    };
  },
  created: function() {
    axios.get("/api/categories/" + this.$route.params.id).then(response => {
      console.log(response.data);
      this.category = response.data;
      this.tasks = response.data.tasks;
    });
  },
  methods: {
    updateCategory: function(category) {
      var params = {
        name: category.name,
        image: category.image
      };
      axios
        .patch("/api/categories/" + this.$route.params.id, params)
        .then(response => {
          console.log(response.data);
          this.$router.push("/");
        })
        .catch(error => {
          this.errors = error.response.data.errors;
        });
    },
    destroyCategory: function(category) {
      axios.delete("api/categories/" + this.$route.params.id).then(response => {
        this.$router.push("/");
      });
    },
    createTask: function() {
      var params = {
        category_id: this.category.id,
        description: this.taskDescription
      };
      axios.post("/api/tasks", params).then(response => {
        console.log(response.data);
        this.tasks.push(response.data);
        this.taskDescription = "";
      });
    }
  }
};
</script>
