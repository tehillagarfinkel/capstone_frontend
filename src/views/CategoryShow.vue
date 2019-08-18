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
          <!--  <div class="col-lg-3 col-sm-6 col-xs-12">
            <div class="pricingTable">
              <div class="priceUper">
                <div class="priceTitle bg-color-2"><h3>New Task</h3></div>
                <div class="priceInfo bg-color-2">
                  <p>Contrary to popular belief, Lorem Ipsum is not simply random text. It has roots.</p>
                </div>
              </div>
              <div class="priceLower">
                <ul class="list-unstyled priceOffer">
                  <li>
                    <i class="fa fa-birthday-cake color-2" aria-hidden="true"></i>
                    2 Meals/Day
                  </li>
                  <li>
                    <i class="fa fa-medkit color-2" aria-hidden="true"></i>
                    Medical Assistance
                  </li>
                  <li>
                    <i class="fa fa-paint-brush color-2" aria-hidden="true"></i>
                    Drowing &amp; Dancing
                  </li>
                  <li>
                    <i class="fa fa-taxi color-2" aria-hidden="true"></i>
                    Free Transport
                  </li>
                </ul>
                <div class="priceBtn">
                  <a href="checkout-step-1.html" class="btn btn-primary bg-color-2">Choose Plan</a>
                  <h4 class="color-2">
                    $69
                    <br />
                    <small>Per Month</small>
                  </h4>
                </div>
              </div>
            </div>
          </div> -->
          <!--  <div class="col-lg-3 col-sm-6 col-xs-12">
            <div class="pricingTable">
              <div class="priceUper">
                <div class="priceTitle bg-color-3"><h3>Fullday Session</h3></div>
                <div class="priceInfo bg-color-3">
                  <p>Contrary to popular belief, Lorem Ipsum is not simply random text. It has roots.</p>
                </div>
              </div>
              <div class="priceLower">
                <ul class="list-unstyled priceOffer">
                  <li>
                    <i class="fa fa-birthday-cake color-3" aria-hidden="true"></i>
                    2 Meals/Day
                  </li>
                  <li>
                    <i class="fa fa-medkit color-3" aria-hidden="true"></i>
                    Medical Assistance
                  </li>
                  <li>
                    <i class="fa fa-paint-brush color-3" aria-hidden="true"></i>
                    Drowing &amp; Dancing
                  </li>
                  <li>
                    <i class="fa fa-taxi color-3" aria-hidden="true"></i>
                    Free Transport
                  </li>
                </ul>
                <div class="priceBtn">
                  <a href="checkout-step-1.html" class="btn btn-primary bg-color-3">Choose Plan</a>
                  <h4 class="color-3">
                    $69
                    <br />
                    <small>Per Month</small>
                  </h4>
                </div>
              </div>
            </div>
          </div>
          <div class="col-lg-3 col-sm-6 col-xs-12">
            <div class="pricingTable">
              <div class="priceUper">
                <div class="priceTitle bg-color-4"><h3>Weekly Session</h3></div>
                <div class="priceInfo bg-color-4">
                  <p>Contrary to popular belief, Lorem Ipsum is not simply random text. It has roots.</p>
                </div>
              </div>
              <div class="priceLower">
                <ul class="list-unstyled priceOffer">
                  <li>
                    <i class="fa fa-birthday-cake color-4" aria-hidden="true"></i>
                    2 Meals/Day
                  </li>
                  <li>
                    <i class="fa fa-medkit color-4" aria-hidden="true"></i>
                    Medical Assistance
                  </li>
                  <li>
                    <i class="fa fa-paint-brush color-4" aria-hidden="true"></i>
                    Drowing &amp; Dancing
                  </li>
                  <li>
                    <i class="fa fa-taxi color-4" aria-hidden="true"></i>
                    Free Transport
                  </li>
                </ul>
                <div class="priceBtn">
                  <a href="checkout-step-1.html" class="btn btn-primary bg-color-4">Choose Plan</a>
                  <h4 class="color-4">
                    $69
                    <br />
                    <small>Per Month</small>
                  </h4>
                </div>
              </div>
            </div>
          </div> -->
        </div>
      </div>
    </section>

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
