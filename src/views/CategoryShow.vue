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
                  <li>
                    <i class="fa fa-taxi color-1" aria-hidden="true"></i>
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
                          <input v-model="task.duration" type="text" />
                        </div>
                        <div>
                          Start Time:
                          <input v-model="task.start_time" type="text" />
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
                    <datetime v-model="taskDueDate"></datetime>
                    <!--  <input v-model="taskDueDate" type="text" /> -->
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

<style></style>

<script>
import axios from "axios";
import { Datetime } from "vue-datetime";

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
    updateTask: function(task) {
      var params = {
        description: task.description,
        duration: task.duration,
        start_time: task.start_time,
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
          task.start_time = "";
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
