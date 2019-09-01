<template>
  <div class="home">
    <section class="py-9 pb-8 bg-parallax" style="background-image: url(/img/event/1.jpg);">
      <!-- <section class="py-9 pb-8 bg-parallax" style="background: black"> -->
      <div class="container">
        <div
          class="sectionTitleSmall text-center mb-7 wow fadeInUp"
          style="visibility: visible; animation-name: fadeInUp;"
        >
          <h2 class="font-weight-bold text-white">My ON IT Stats</h2>
        </div>

        <div class="row wow fadeInUp" id="counter" style="visibility: visible; animation-name: fadeInUp;">
          <div class="col-sm-3 col-xs-12">
            <div class="text-center text-white mb-5">
              <div class="counter-value" data-count="179">179</div>
              <span
                class="d-inline-block bg-warning text-uppercase font-weight-medium rounded-sm shadow-sm mt-1 py-2 px-3"
              >
                Categories
              </span>
            </div>
          </div>

          <div class="col-sm-3 col-xs-12">
            <div class="text-center text-white mb-5">
              <div class="counter-value" data-count="548">548</div>
              <span
                class="d-inline-block bg-success text-uppercase font-weight-medium rounded-sm shadow-sm mt-1 py-2 px-3"
              >
                Tasks
              </span>
            </div>
          </div>

          <div class="col-sm-3 col-xs-12">
            <div class="text-center text-white mb-5">
              <div class="counter-value" data-count="305">305</div>
              <span
                class="d-inline-block bg-danger text-uppercase font-weight-medium rounded-sm shadow-sm mt-1 py-2 px-3"
              >
                Tasks Completed
              </span>
            </div>
          </div>

          <div class="col-sm-3 col-xs-12">
            <div class="text-center text-white mb-5">
              <div class="counter-value" data-count="1000">1000</div>
              <span
                class="d-inline-block bg-info text-uppercase font-weight-medium rounded-sm shadow-sm mt-1 py-2 px-3"
              >
                Tasks to do
              </span>
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
                <i :class="`fa fa-graduation-cap bg-color-${index + 1}`" aria-hidden="true"></i>
              </span>
              <div class="media-body">
                <h3 :class="`media-heading color-${index + 1}`">{{ category.name }}</h3>
                <div>
                  <router-link v-bind:to="`/category/${category.id}`">View tasks</router-link>
                </div>
                <p></p>
              </div>
            </div>
          </div>
          <div class="col-sm-4 col-xs-12">
            <div class="media featuresContent">
              <span class="media-left bg-color-5">
                <i class="fa fa-plus" aria-hidden="true"></i>
              </span>
              <div class="media-body">
                <h3 class="media-heading color-5">New Category</h3>
                <div>
                  Name:
                  <input v-model="categoryName" type="text" />
                </div>
                <div>
                  Image:
                  <input v-model="categoryImage" type="text" />
                  <div>
                    <button v-on:click="createCategory" type="button" class="btn btn-success btn-sm">Add</button>
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
.bg-parallax {
  background-size: cover;
  background-position: center;
  background-attachment: fixed;
  background-color: "#404040";
}

.bg-repeat {
  background-repeat: repeat;
}

#counter .counter-value {
  width: 145px;
  height: 145px;
  line-height: 145px;
  border-radius: 100%;
  border: 4px solid #fff;
  display: block;
  margin: 0 auto 22px;
  font-size: 2rem;
}

@media (min-width: 768px) {
  #counter .counter-value {
    font-size: 3.25rem;
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
</style>

<script>
/* global $ */
import axios from "axios";

export default {
  data: function() {
    return {
      categories: [],
      currentCategory: {},
      categoryName: "",
      categoryImage: ""
    };
  },
  created: function() {
    axios.get("/api/categories").then(response => {
      console.log(response.data);
      this.categories = response.data;
    });
  },
  mounted: function() {
    $(".counter").counterUp({
      delay: 10,
      time: 2000
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
