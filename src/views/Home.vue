<template>
  <div class="home">
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
          <div v-for="category in categories" class="col-sm-4 col-xs-12">
            <div class="media featuresContent">
              <span class="media-left bg-color-1">
                <i class="fa fa-graduation-cap bg-color-1" aria-hidden="true"></i>
              </span>
              <div class="media-body">
                <h3 class="media-heading color-1">{{ category.name }}</h3>
                <router-link v-bind:to="`/category/${category.id}`">View {{ category.name }} tasks</router-link>
                <p></p>
              </div>
            </div>
          </div>
          <div class="col-sm-4 col-xs-12">
            <div class="media featuresContent">
              <span class="media-left bg-color-2">
                <i class="fa fa-leaf bg-color-2" aria-hidden="true"></i>
              </span>
              <div class="media-body">
                <h3 class="media-heading color-2">New Category</h3>
                <div>
                  Name:
                  <input v-model="categoryName" type="text" />
                </div>
                <div>
                  Image:
                  <input v-model="categoryImage" type="text" />
                  <div>
                    <button v-on:click="createCategory">Add</button>
                  </div>
                </div>
              </div>
            </div>
          </div>
          <!-- <div class="col-sm-4 col-xs-12">
            <div class="media featuresContent">
              <span class="media-left bg-color-3">
                <i class="fa fa-car bg-color-3" aria-hidden="true"></i>
              </span>
              <div class="media-body">
                <h3 class="media-heading color-3">Physical Activity</h3>
                <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt.</p>
              </div>
            </div>
          </div>
          <div class="col-sm-4 col-xs-12">
            <div class="media featuresContent">
              <span class="media-left bg-color-4">
                <i class="fa fa-cutlery bg-color-4" aria-hidden="true"></i>
              </span>
              <div class="media-body">
                <h3 class="media-heading color-4">Delicious Food</h3>
                <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt.</p>
              </div>
            </div>
          </div>
          <div class="col-sm-4 col-xs-12">
            <div class="media featuresContent">
              <span class="media-left bg-color-5">
                <i class="fa fa-heart bg-color-5" aria-hidden="true"></i>
              </span>
              <div class="media-body">
                <h3 class="media-heading color-5">Love &amp; Care</h3>
                <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt.</p>
              </div>
            </div>
          </div>
          <div class="col-sm-4 col-xs-12">
            <div class="media featuresContent">
              <span class="media-left bg-color-6">
                <i class="fa fa-shield bg-color-6" aria-hidden="true"></i>
              </span>
              <div class="media-body">
                <h3 class="media-heading color-6">Meny Sports</h3>
                <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt.</p>
              </div>
            </div>
          </div> -->
        </div>
      </div>
    </section>
</template>

<style></style>

<script>
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
