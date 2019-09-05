<template>
  <div class="home">
    <section
      class="py-9 pb-8 bg-parallax"
      style="background-image: url('https://www.beesapps.com/wp-content/uploads/2016/04/sticky-notes-1.png')"
    >
      <div class="container">
        <div
          class="sectionTitleSmall text-center mb-7 wow fadeInUp"
          style="visibility: visible; animation-name: fadeInUp;"
        >
          <h2 class="dashboard-header">My ON IT Stats</h2>
          <!--  <h2>
            <span class="shape shape-left bg-color-4"></span>
            <span>My ON IT Stats</span>
            <span class="shape shape-right bg-color-4"></span>
          </h2> -->
        </div>

        <div class="row wow fadeInUp" id="counter" style="visibility: visible; animation-name: fadeInUp;">
          <div class="col-sm-3 col-xs-12">
            <div class="text-center text-white mb-5">
              <div id="count-categories" class="counter-value"></div>
              <button id="dashboard-button" type="button" class="btn btn-light color-5">Categories</button>
            </div>
          </div>

          <div class="col-sm-3 col-xs-12">
            <div class="text-center text-white mb-5">
              <div id="count-tasks" class="counter-value"></div>
              <button id="dashboard-button" type="button" class="btn btn-light color-6" @click="$router.push('/tasks')">
                Tasks
              </button>
            </div>
          </div>

          <div class="col-sm-3 col-xs-12">
            <div class="text-center text-white mb-5">
              <div id="count-events" class="counter-value"></div>
              <button
                id="dashboard-button"
                type="button"
                class="btn btn-light color-1"
                @click="$router.push('/calendar')"
              >
                Events
              </button>
            </div>
          </div>

          <div class="col-sm-3 col-xs-12">
            <div class="text-center text-white mb-5">
              <div id="count-tasks-todo" class="counter-value"></div>
              <button id="dashboard-button" type="button" class="btn btn-light color-4" @click="$router.push('/tasks')">
                Tasks To Do
              </button>
            </div>
          </div>
        </div>
      </div>
    </section>

    <section class="mainContent full-width clearfix featureSection">
      <div class="container">
        <div class="sectionTitle text-center">
          <h2>
            <span class="shape shape-left bg-color-4"></span>
            <span>My Categories</span>
            <span class="shape shape-right bg-color-4"></span>
          </h2>
        </div>

        <div class="row">
          <div v-for="(category, index) in categories" class="col-sm-4 col-xs-12">
            <div class="media featuresContent">
              <span :class="`media-left bg-color-${index + 1}`">
                <i :class="`fa fa-spinner bg-color-${index + 1}`" aria-hidden="true"></i>
              </span>
              <div class="media-body">
                <h3 :class="`media-heading color-${index + 1}`">{{ category.name }}</h3>
                <div>
                  <router-link v-bind:to="`/category/${category.id}`" :class="`media-heading color-${index + 1}`">
                    View tasks
                  </router-link>
                </div>
                <p></p>
              </div>
            </div>
          </div>
          <div class="col-sm-4 col-xs-12">
            <div class="media featuresContent">
              <span class="media-left bg-color-3">
                <i class="fa fa-plus" aria-hidden="true"></i>
              </span>
              <div class="media-body">
                <h3 class="media-heading color-3">New Category</h3>

                <div class="form-group">
                  <input
                    type="text"
                    v-model="categoryName"
                    class="form-control border-color-3"
                    id="exampleInputEmail1"
                    placeholder="Name"
                  />
                </div>

                <div class="form-group">
                  <input
                    type="text"
                    v-model="categoryImage"
                    class="form-control border-color-3"
                    id="exampleInputEmail1"
                    placeholder="Image"
                  />
                </div>

                <div>
                  <div>
                    <button v-on:click="createCategory" type="button" class=" btn btn-primary btn-sm bg-color-3">
                      Add
                    </button>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>
  </div>
</template>

<style scoped>
#count-tasks-todo,
#count-events,
#count-tasks,
#count-categories {
  color: white;
}
.bg-parallax {
  background-size: cover;
  background-position: center;
  background-attachment: fixed;
  background-color: "#404040";
  padding-top: 15px;
  padding-bottom: 15px;
}

.bg-repeat {
  background-repeat: repeat;
}

#counter .counter-value {
  width: 175px;
  height: 175px;
  line-height: 175px;
  border-radius: 100%;
  border: 6px solid #fff;
  display: block;
  margin: 0 auto 22px;
  font-size: 2rem;
}

@media (min-width: 768px) {
  #counter .counter-value {
    font-size: 7rem;
    font-family: "Dosis", sans-serif;
    font-weight: bold;
  }
}

#counter span {
  font-size: 0.8125rem;
}

@media (min-width: 768px) {
  #counter span {
    font-size: 0.9375rem;
  }
}
.text-center {
  text-align: center !important;
}

.d-md-inline-block {
  display: inline-block !important;
}

.dashboard-header {
  font-size: 50px;
}

#dashboard-button {
  font-family: "Dosis", sans-serif;
  font-size: 25px;
}
</style>

<script>
/* global $ */
import axios from "axios";
import { CountUp } from "countup.js";

export default {
  data: function() {
    return {
      categories: [],
      currentCategory: {},
      categoryName: "",
      categoryImage: ""
    };
  },
  created: function() {},
  mounted: function() {
    axios.get("/api/categories").then(response => {
      console.log(response.data);
      this.categories = response.data;

      const options = {
        duration: 15
      };

      const countUpCategories = new CountUp("count-categories", 6);
      countUpCategories.start();

      const countUpTasks = new CountUp("count-tasks", 34);
      countUpTasks.start();

      const countUpEvents = new CountUp("count-events", 67);
      countUpEvents.start();

      const countUpTasksToDo = new CountUp("count-tasks-todo", 12);
      countUpTasksToDo.start();
    });
  },
  methods: {
    createCategory: function() {
      var params = {
        name: this.categoryName,
        image: this.categoryImage
      };
      axios.post("/api/categories", params).then(response => {
        console.log(response.data);
        this.categories.push(response.data);
        this.categoryName = "";
        this.categoryImage = "";
      });
    }
  }
};
</script>
