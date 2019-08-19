<template>
  <div class="home">
    <section class="mainContent full-width clearfix">
      <div class="container">
        <div class="sectionTitle text-center">
          <h2>
            <span class="shape shape-left bg-color-4"></span>
            <span>My {{ category.name }} tasks</span>
            <span class="shape shape-right bg-color-4"></span>
          </h2>
        </div>

        <div class="row">
          <div v-for="task in tasks" class="col-lg-3 col-sm-6 col-xs-12">
            <div class="pricingTable">
              <div class="priceUper">
                <div class="priceTitle bg-color-1">
                  <h3>{{ task.description }}</h3>
                </div>
              </div>
              <div class="priceLower">
                <ul class="list-unstyled priceOffer">
                  <li>
                    <i class="fa fa-taxi color-1" aria-hidden="true"></i>
                    Completed? {{ task.completed }}
                  </li>
                  <li>
                    <i class="fa fa-birthday-cake color-1" aria-hidden="true"></i>
                    {{ task.duration }} minutes
                  </li>
                  <li>
                    <i class="fa fa-medkit color-1" aria-hidden="true"></i>
                    Due: {{ task.due_date }}
                  </li>
                  <li>
                    <i class="fa fa-paint-brush color-1" aria-hidden="true"></i>
                    Start time: {{ task.start_time }}
                  </li>
                </ul>
                <div class="priceBtn">
                  <div class="btn btn-primary bg-color-1">
                    <router-link v-bind:to="`/tasks/${task.id}`">More info</router-link>
                  </div>
                </div>
              </div>
            </div>
          </div>
          <div class="col-lg-3 col-sm-6 col-xs-12">
            <div class="pricingTable">
              <div class="priceUper">
                <div class="priceTitle bg-color-2">
                  <h3>New Task</h3>
                </div>
              </div>
              <div class="priceLower">
                <ul class="list-unstyled priceOffer">
                  <li>
                    <i class="fa fa-taxi color-2" aria-hidden="true"></i>
                    Description:
                    <input v-model="taskDescription" type="text" />
                  </li>
                  <li>
                    <i class="fa fa-birthday-cake color-2" aria-hidden="true"></i>
                    Duration:
                    <input v-model="taskDuration" type="text" />
                  </li>
                  <li>
                    <i class="fa fa-medkit color-2" aria-hidden="true"></i>
                    Due Date:
                    <input v-model="taskDueDate" type="text" />
                  </li>
                  <li>
                    <i class="fa fa-paint-brush color-2" aria-hidden="true"></i>
                    Start time:
                    <input v-model="taskStartTime" type="text" />
                  </li>
                </ul>
                <div class="priceBtn">
                  <div v-on:click="createTask" class="btn btn-primary bg-color-2">
                    Create Task
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>

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
      category: [],
      tasks: [],
      name: "",
      image: "",
      taskDescription: "",
      taskDuration: "",
      taskDueDate: "",
      taskStartTime: ""
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
        description: this.taskDescription,
        duration: this.taskDuration,
        due_date: this.taskDueDate,
        start_time: this.taskStartTime
      };
      axios.post("/api/tasks", params).then(response => {
        console.log(response.data);
        this.tasks.push(response.data);
        this.taskDescription = "";
        this.taskDuration = "";
        this.taskDueDate = "";
        this.taskStartTime = "";
      });
    }
  }
};
</script>
