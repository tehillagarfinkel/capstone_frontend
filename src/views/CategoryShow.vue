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
          <div v-for="task in orderBy(tasks, 'completed')" class="col-lg-3 col-sm-6 col-xs-12">
            <div class="pricingTable" v-bind:class="{ completed: task.completed }">
              <div class="priceUper">
                <div class="priceTitle bg-color-1">
                  <h3>{{ task.description }}</h3>
                </div>
              </div>
              <div class="priceLower">
                <ul class="list-unstyled priceOffer">
                  <li>
                    <i class="fa fa-clock-o color-1" aria-hidden="true"></i>
                    {{ task.duration }} minutes
                  </li>
                  <li>
                    <i class="fa  fa-calendar color-1" aria-hidden="true"></i>
                    Due: {{ task.formatted.due_date }}
                  </li>
                  <li>
                    <i class="fa fa-hourglass-start color-1" aria-hidden="true"></i>
                    Start time: {{ task.formatted.start }}
                  </li>
                  <li>
                    <i class="fa  fa-check-square-o color-1" aria-hidden="true"></i>
                    Completed: {{ task.completed }}
                    <div>
                      Completed
                      <input v-model="task.completed" v-on:change="markComplete(task)" type="checkbox" />
                    </div>
                  </li>
                  <li>
                    <i class="fa fa-calendar color-1" aria-hidden="true"></i>
                    <router-link v-bind:to="`/calendar`">View in calendar</router-link>
                  </li>
                </ul>
                <button
                  type="button"
                  class="btn btn-primary bg-color-1"
                  data-toggle="modal"
                  :data-target="`#exampleModal${task.id}`"
                >
                  Edit
                </button>
                <div
                  class="modal fade mb-8"
                  :id="`exampleModal${task.id}`"
                  tabindex="-1"
                  role="dialog"
                  aria-labelledby="exampleModalLabel"
                  aria-hidden="true"
                >
                  <div class="modal-dialog" role="document">
                    <div class="modal-content">
                      <div class="modal-header">
                        <h2 class="modal-title">{{ task.description }}</h2>
                        <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                          <span aria-hidden="true">&times;</span>
                        </button>
                      </div>
                      <div class="modal-body">
                        <div class="form-group">
                          <i class="fa fa-pencil-square-o"></i>
                          Description:
                          <input
                            type="text"
                            v-model="task.description"
                            class="form-control border-color-1"
                            placeholder="Description"
                          />
                        </div>

                        <div class="form-group">
                          <i class="fa fa-clock-o"></i>
                          Duration:
                          <input
                            type="text"
                            v-model="task.duration"
                            class="form-control border-color-1"
                            placeholder="Duration"
                          />
                        </div>

                        <div class="form-group">
                          <i class="fa fa-hourglass-start"></i>
                          Start Time:
                          <input
                            type="text"
                            v-model="task.start"
                            class="form-control border-color-1"
                            placeholder="Start Time"
                          />
                        </div>

                        <div class="form-group">
                          <i class="fa fa-calendar"></i>
                          Due Date:
                          <datetime
                            v-model="task.due_date"
                            class="form-control border-color-1"
                            placeholder="Due Date"
                          />
                        </div>

                        <!--  <div>
                          Due Date:
                          <datetime v-model="task.due_date"></datetime>
                        </div> -->
                      </div>
                      <div class="modal-footer">
                        <button
                          type="button"
                          class=" btn btn-primary bg-color-3"
                          data-dismiss="modal"
                          v-on:click="destroyTask(task)"
                        >
                          Delete Task
                        </button>
                        <button
                          type="button"
                          class="btn btn-primary bg-color-1"
                          data-dismiss="modal"
                          v-on:click="updateTask(task)"
                        >
                          Save changes
                        </button>
                      </div>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
        <div class="row">
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
                    <i class="fa fa-pencil-square-o color-2" aria-hidden="true"></i>
                    Description:
                    <input v-model="taskDescription" type="text" />
                  </li>
                  <li>
                    <i class="fa fa-clock-o color-2" aria-hidden="true"></i>
                    Duration:
                    <input type="number" v-model="taskDuration" />
                  </li>
                  <li>
                    <i class="fa fa-calendar color-2" aria-hidden="true"></i>
                    Due Date:
                    <datetime v-model="taskDueDate"></datetime>
                  </li>
                  <li>
                    <i class="fa fa-hourglass-start color-2" aria-hidden="true"></i>
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

    <div><a href="/category" class="btn btn-primary btn-lg" role="button">Back to my categories</a></div>
    <div>
      Name:
      <input v-model="category.name" type="text" />
      Image:
      <input v-model="category.image" type="text" />
      <button v-on:click="updateCategory(category)">Update Category</button>
      <button v-on:click="destroyCategory(category)">Delete Category</button>
    </div>
  </div>
</template>

<style>
.completed {
  opacity: 0.5;
}
</style>

<script>
import axios from "axios";
import { Datetime } from "vue-datetime";
import Vue2Filters from "vue2-filters";

export default {
  mixins: [Vue2Filters.mixin],
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
  computed: {
    incompleteTasks: function() {
      return this.tasks.filter(task => !task.completed);
    },
    completeTasks: function() {
      return this.tasks.filter(task => task.completed);
    }
  },
  created: function() {
    axios.get("/api/categories/" + this.$route.params.id).then(response => {
      console.log(response.data);
      this.category = response.data;
      this.tasks = response.data.tasks;

      // var categories = response.data;
      // categories.forEach(category => {
      //   category.tasks.forEach(task => {
      //     this.tasks.push(task);
      //   });
      // });
    });
  },
  methods: {
    updateTask: function(task) {
      var params = {
        description: task.description,
        duration: task.duration,
        start: task.start,
        due_date: task.due_date
      };
      Object.keys(params).forEach(key => params[key] === "" && delete params[key]);
      axios
        .patch("/api/tasks/" + task.id, params)
        .then(response => {
          console.log(response.data);
          task = response.data;
          task.description = "";
          task.duration = "";
          task.start = "";
          task.due_date = "";
        })
        .catch(error => {
          this.errors = error.response.data.errors;
        });
    },
    destroyTask: function(task) {
      axios.delete("/api/tasks/" + task.id).then(response => {
        this.$router.push(`/category/${task.category_id}`);
      });
    },
    updateCategory: function(category) {
      var params = {
        name: category.name,
        image: category.image
      };
      axios
        .patch("/api/categories/" + this.$route.params.id, params)
        .then(response => {
          console.log(response.data);
          this.$router.push("/category");
        })
        .catch(error => {
          this.errors = error.response.data.errors;
        });
    },
    destroyCategory: function(category) {
      axios.delete("api/categories/" + this.$route.params.id).then(response => {
        this.$router.push("/category");
      });
    },
    createTask: function() {
      var params = {
        category_id: this.category.id,
        description: this.taskDescription,
        duration: this.taskDuration,
        due_date: this.taskDueDate,
        start: this.taskStartTime
      };
      axios.post("/api/tasks", params).then(response => {
        console.log(response.data);
        this.tasks.push(response.data);
        this.taskDescription = "";
        this.taskDuration = "";
        this.taskDueDate = "";
        this.taskStartTime = "";
      });
    },
    markComplete: function(task) {
      // if (task.completed === "true") {
      //   task.completed = "false";
      // } else {
      //   task.completed = "true";
      // }
      console.log("markComplete", task, task.completed, !task.completed);
      var params = {
        completed: task.completed
      };
      // Object.keys(params).forEach(key => params[key] === "" && delete params[key]);
      axios
        .patch("/api/tasks/" + task.id, params)
        .then(response => {
          console.log(response.data);
          // task = response.data;
          // task.completed = response.data.completed;
          // task.completed = "";
        })
        .catch(error => {
          this.errors = error.response.data.errors;
        });
    }
  }
};
</script>
