<template>
  <div class="home">
    <section class="mainContent full-width clearfix">
      <div class="container">
        <div class="sectionTitle text-center">
          <h2>
            <span class="shape shape-left bg-color-4"></span>
            <span>All Tasks</span>
            <span class="shape shape-right bg-color-4"></span>
          </h2>
          <div class="input-group searchArea">
            <span class="input-group-addon"><i class="fa fa-search" aria-hidden="true"></i></span>
            <input
              type="text"
              v-model="searchFilter"
              class="form-control"
              aria-label="Amount (to the nearest dollar)"
              placeholder="Find a task..."
            />
            <button type="submit" class="input-group-addon button" id="basic-addon21">Search</button>
          </div>
        </div>

        <div class="row">
          <div v-for="task in filterBy(tasks, searchFilter, 'description')" class="col-lg-3 col-sm-6 col-xs-12">
            <div class="pricingTable">
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
                    Due: {{ task.due_date }}
                  </li>
                  <li>
                    <i class="fa fa-paint-brush color-1" aria-hidden="true"></i>
                    Start time: {{ task.start }}
                  </li>
                  <li>
                    <i class="fa  fa-check-square-o color-1" aria-hidden="true"></i>
                    Completed: {{ task.completed }}
                    <div>
                      Completed
                      <input v-on:click="markComplete(task)" type="checkbox" />
                    </div>
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
                        <div>
                          Description:
                          <input v-model="task.description" type="text" />
                        </div>
                        <div>
                          Duration:
                          <input type="number" v-model="task.duration" />
                        </div>
                        <div>
                          Start Time:
                          <input v-model="task.start" type="text" />
                        </div>
                        <div>
                          Due Date:
                          <datetime v-model="task.due_date"></datetime>
                        </div>
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

        <div>
          <button type="button" class="btn btn-primary" @click="$router.push('/category')">
            Back to my Categories
          </button>
        </div>
      </div>
    </section>
  </div>
</template>

<style></style>

<script>
import axios from "axios";
import { Datetime } from "vue-datetime";
import Vue2Filters from "vue2-filters";

export default {
  mixins: [Vue2Filters.mixin],
  data: function() {
    return {
      searchFilter: "",
      tasks: [],
      name: "",
      image: "",
      taskCategory: "",
      taskDescription: "",
      taskDuration: "",
      taskDueDate: "",
      taskStartTime: ""
    };
  },
  created: function() {
    axios.get("/api/categories/").then(response => {
      console.log(response.data);
      var categories = response.data;
      categories.forEach(category => {
        category.tasks.forEach(task => {
          this.tasks.push(task);
        });
      });
      console.log("tasks are", this.tasks);
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
    createTask: function() {
      var params = {
        category_id: this.taskCategory,
        description: this.taskDescription,
        duration: this.taskDuration,
        due_date: this.taskDueDate,
        start: this.taskStartTime
      };
      axios.post("/api/tasks", params).then(response => {
        console.log(response.data);
        this.tasks.push(response.data);
        this.taskCategory = "";
        this.taskDescription = "";
        this.taskDuration = "";
        this.taskDueDate = "";
        this.taskStartTime = "";
      });
    },
    markComplete: function(task) {
      if (task.completed === "true") {
        task.completed = "false";
      } else {
        task.completed = "true";
      }

      var params = {
        completed: task.completed
      };
      Object.keys(params).forEach(key => params[key] === "" && delete params[key]);
      axios
        .patch("/api/tasks/" + task.id, params)
        .then(response => {
          console.log(response.data);
          task = response.data;
          task.completed = "";
        })
        .catch(error => {
          this.errors = error.response.data.errors;
        });
    }
  }
};
</script>
