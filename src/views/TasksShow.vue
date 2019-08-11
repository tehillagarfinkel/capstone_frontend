<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <h1>{{ task.description }}</h1>
    <p>Duration: {{ task.duration }}</p>
    <p>Start Time: {{ task.start_time }}</p>
    <p>Due Date: {{ task.due_date }}</p>
    <p>Completed: {{ task.completed }}</p>
    <div>
      Description:
      <input v-model="description" type="text" />
      Duration:
      <input v-model="duration" type="text" />
      Start Time:
      <input v-model="startTime" type="text" />
      Due Date:
      <input v-model="dueDate" type="text" />
      Completed:
      <input v-model="completed" type="text" />

      <button v-on:click="updateTask(task)">Update task information</button>
      <button v-on:click="destroyTask(task)">Delete Task</button>
    </div>
    <router-link v-bind:to="`/category/${task.category_id}`">Back to all tasks</router-link>
  </div>
</template>

<style></style>

<script>
import axios from "axios";
export default {
  data: function() {
    return {
      message: "My Task:",
      task: {},
      description: "",
      duration: "",
      startTime: "",
      dueDate: "",
      completed: ""
    };
  },
  created: function() {
    axios.get("/api/tasks/" + this.$route.params.id).then(response => {
      this.task = response.data;
    });
  },
  methods: {
    updateTask: function(task) {
      var params = {
        description: this.description,
        duration: this.duration,
        start_time: this.startTime,
        due_date: this.dueDate,
        completed: this.completed
      };
      Object.keys(params).forEach(key => params[key] === "" && delete params[key]);
      axios
        .patch("/api/tasks/" + this.$route.params.id, params)
        .then(response => {
          console.log(response.data);
          this.task = response.data;
          this.description = "";
          this.duration = "";
          this.startTime = "";
          this.dueDate = "";
          this.completed = "";
        })
        .catch(error => {
          this.errors = error.response.data.errors;
        });
    },
    destroyTask: function(task) {
      axios.delete("/api/tasks/" + this.$route.params.id).then(response => {
        this.$router.push(`/category/${task.category_id}`);
      });
    }
  }
};
</script>
