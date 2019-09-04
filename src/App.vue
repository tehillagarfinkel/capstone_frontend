<template>
  <div id="app">
    <header id="pageTop" class="header-wrapper">
      <!-- COLOR BAR -->
      <div class="container-fluid color-bar top-fixed clearfix">
        <div class="row">
          <div class="col-sm-1 col-xs-2 bg-color-1">fix bar</div>
          <div class="col-sm-1 col-xs-2 bg-color-2">fix bar</div>
          <div class="col-sm-1 col-xs-2 bg-color-3">fix bar</div>
          <div class="col-sm-1 col-xs-2 bg-color-4">fix bar</div>
          <div class="col-sm-1 col-xs-2 bg-color-5">fix bar</div>
          <div class="col-sm-1 col-xs-2 bg-color-6">fix bar</div>
          <div class="col-sm-1 bg-color-1 hidden-xs">fix bar</div>
          <div class="col-sm-1 bg-color-2 hidden-xs">fix bar</div>
          <div class="col-sm-1 bg-color-3 hidden-xs">fix bar</div>
          <div class="col-sm-1 bg-color-4 hidden-xs">fix bar</div>
          <div class="col-sm-1 bg-color-5 hidden-xs">fix bar</div>
          <div class="col-sm-1 bg-color-6 hidden-xs">fix bar</div>
        </div>
      </div>

      <!-- TOP INFO BAR -->
      <div class="top-info-bar bg-color-7 hidden-xs">
        <div class="container">
          <div class="row">
            <div class="col-sm-5">
              <ul class="list-inline functionList">
                <li>
                  <i class="fa fa-unlock-alt bg-color-5" aria-hidden="true"></i>
                  <a href="/login" data-toggle="modal">Login</a>

                  <i class="fa fa-plus-circle bg-color-5" aria-hidden="true"></i>
                  <a href="/signup" data-toggle="modal">Create an account</a>
                  <i class="fa fa-lock bg-color-5" aria-hidden="true"></i>
                  <a href="/logout" data-toggle="modal">Logout</a>
                </li>
              </ul>
            </div>
          </div>
        </div>
      </div>

      <!-- NAVBAR -->
      <nav id="menuBar" class="navbar navbar-default lightHeader" role="navigation">
        <div class="container">
          <!-- Brand and toggle get grouped for better mobile display -->
          <div class="navbar-header">
            <button type="button" class="navbar-toggle" data-toggle="collapse" data-target=".navbar-ex1-collapse">
              <span class="sr-only">Toggle navigation</span>
              <span class="icon-bar"></span>
              <span class="icon-bar"></span>
              <span class="icon-bar"></span>
            </button>
            <a class="navbar-brand" href="/category"><img src="./assets/onitlogoheader.png" alt="ON IT!" /></a>
          </div>

          <!-- Collect the nav links, forms, and other content for toggling -->
          <div class="collapse navbar-collapse navbar-ex1-collapse">
            <ul class="nav navbar-nav navbar-right">
              <li class="dropdown singleDrop color-1   active ">
                <a href="/category" role="button" aria-haspopup="true" aria-expanded="false">
                  <i class="fa fa-home bg-color-1" aria-hidden="true"></i>
                  <span class="active">Home</span>
                </a>
              </li>
              <li class="dropdown singleDrop color-3 ">
                <a
                  href="/"
                  class="dropdown-toggle"
                  data-toggle="dropdown"
                  role="button"
                  aria-haspopup="true"
                  aria-expanded="false"
                >
                  <i class="fa fa-list-ul bg-color-3" aria-hidden="true"></i>
                  <span>Categories</span>
                </a>
                <ul class="dropdown-menu">
                  <li><a href="/category">All Categories</a></li>
                  <li class="" v-for="category in categories">
                    <a v-bind:href="`/category/${category.id}`">{{ category.name }}</a>
                  </li>
                </ul>
              </li>

              <li class="dropdown singleDrop color-2 ">
                <a href="/tasks" role="button" aria-haspopup="true" aria-expanded="false">
                  <i class="fa fa-check-square-o bg-color-2" aria-hidden="true"></i>
                  <span>Tasks</span>
                </a>
              </li>

              <li class="dropdown singleDrop color-4 ">
                <a href="/calendar" role="button" aria-haspopup="true" aria-expanded="false">
                  <i class="fa fa-calendar bg-color-4" aria-hidden="true"></i>
                  <span>Calendar</span>
                </a>
              </li>
            </ul>
          </div>
        </div>
      </nav>
    </header>

    <router-view />

    <footer>
      <!-- COLOR BAR -->
      <div class="container-fluid color-bar clearfix">
        <div class="row">
          <div class="col-sm-1 col-xs-2 bg-color-1">fix bar</div>
          <div class="col-sm-1 col-xs-2 bg-color-2">fix bar</div>
          <div class="col-sm-1 col-xs-2 bg-color-3">fix bar</div>
          <div class="col-sm-1 col-xs-2 bg-color-4">fix bar</div>
          <div class="col-sm-1 col-xs-2 bg-color-5">fix bar</div>
          <div class="col-sm-1 col-xs-2 bg-color-6">fix bar</div>
          <div class="col-sm-1 bg-color-1 hidden-xs">fix bar</div>
          <div class="col-sm-1 bg-color-2 hidden-xs">fix bar</div>
          <div class="col-sm-1 bg-color-3 hidden-xs">fix bar</div>
          <div class="col-sm-1 bg-color-4 hidden-xs">fix bar</div>
          <div class="col-sm-1 bg-color-5 hidden-xs">fix bar</div>
          <div class="col-sm-1 bg-color-6 hidden-xs">fix bar</div>
        </div>
      </div>

      <!-- COPY RIGHT -->
      <div class="copyRight clearfix">
        <div class="container">
          <div class="row">
            <div class="col-sm-5 col-sm-push-7 col-xs-12">
              <ul class="list-inline">
                <li>
                  <a href="#" class="bg-color-2">
                    <i class="fa fa-clock-o" aria-hidden="true"></i>
                  </a>
                </li>
                <!-- <li>
                  <a href="#" class="bg-color-2"><i class="fa fa-twitter" aria-hidden="true"></i></a>
                </li>
                <li>
                  <a href="#" class="bg-color-3"><i class="fa fa-google-plus" aria-hidden="true"></i></a>
                </li>
                <li>
                  <a href="#" class="bg-color-4"><i class="fa fa-pinterest-p" aria-hidden="true"></i></a>
                </li>
                <li>
                  <a href="#" class="bg-color-5"><i class="fa fa-vimeo" aria-hidden="true"></i></a>
                </li> -->
              </ul>
            </div>
            <div class="col-sm-7 col-sm-pull-5 col-xs-12">
              <div class="copyRightText"></div>
            </div>
          </div>
        </div>
      </div>
    </footer>
  </div>
</template>

<style>
img {
  padding: 5px;
  width: 20px;
  height: 125px;
}

.navbar-brand img {
  width: 325px !important;
  height: 100px;
  padding: 0 !important;
  margin: -28px;
}
</style>

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
      this.categories = response.data;
    });
  }
};
</script>
