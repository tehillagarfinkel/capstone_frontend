<template>
  <div class="home">
    <div>
      Description:
      <input v-model="description" type="text" />
      Duration:
      <input v-model="duration" type="text" />
      Start Time:
      <input v-model="startTime" type="text" />
      Due Date:
      <input v-model="dueDate" type="text" />

      <button v-on:click="updateTask(task)">Update task information</button>
      <div>
        <button v-on:click="destroyTask(task)">Delete Task</button>
      </div>
      <div>
        Task Completed
        <input v-on:click="markComplete(task)" type="checkbox" />
      </div>
    </div>
    <router-link v-bind:to="`/category/${task.category_id}`">Back to all tasks</router-link>

    <section class="py-7 py-md-10">
      <div class="container">
        <div class="row">
          <div class="col-sm-4 col-xs-12">
            <div class="image mb-5 mb-md-0">
              <img class="w-100 rounded" src="assets/img/features/features-team-1.jpg" alt="team-1.jpg" />
            </div>
          </div>

          <div class="col-sm-8 col-xs-12">
            <h2 class="text-danger font-weight-bold text-capitalize pl-0 mb-5">My Task</h2>

            <h2 class="text-danger font-weight-medium mb-3 ">{{ task.description }}</h2>

            <div class="text-white rounded bg-warning text-uppercase font-weight-medium px-6 py-3 mb-3">
              Duration:
            </div>

            <div class="text-muted text-capitalize font-weight-medium ml-4 mb-5 font-size-20">
              {{ task.duration }} minutes
            </div>

            <div class="text-white rounded bg-success text-uppercase font-weight-medium px-6 py-3 mb-3">
              Start Time:
            </div>

            <div class="text-muted text-capitalize font-weight-medium ml-4 mb-5 font-size-20">
              {{ task.start_time }}
            </div>

            <div class="text-white rounded bg-danger text-uppercase font-weight-medium px-6 py-3 mb-3">Due Date:</div>

            <div class="text-muted text-capitalize font-weight-medium ml-4 mb-5 font-size-20">
              {{ task.due_date }}
            </div>

            <div class="text-white rounded bg-danger text-uppercase font-weight-medium px-6 py-3 mb-3">Completed:</div>

            <div class="text-muted text-capitalize font-weight-medium ml-4 mb-5 font-size-20">
              {{ task.completed }}
            </div>
          </div>
        </div>
      </div>
    </section>
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
        due_date: this.dueDate
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
        })
        .catch(error => {
          this.errors = error.response.data.errors;
        });
    },
    destroyTask: function(task) {
      axios.delete("/api/tasks/" + this.$route.params.id).then(response => {
        this.$router.push(`/category/${task.category_id}`);
      });
    },
    markComplete: function(task) {
      if (this.completed === "true") {
        this.completed = "false";
      } else {
        this.completed = "true";
      }

      var params = {
        completed: this.completed
      };
      Object.keys(params).forEach(key => params[key] === "" && delete params[key]);
      axios
        .patch("/api/tasks/" + this.$route.params.id, params)
        .then(response => {
          console.log(response.data);
          this.task = response.data;
          this.completed = "";
        })
        .catch(error => {
          this.errors = error.response.data.errors;
        });
    }
  }
};
</script>
