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
            <a class="navbar-brand" href="index.html"><img src="on it logo draft 1.png" alt="ON IT!" /></a>
          </div>

          <!-- Collect the nav links, forms, and other content for toggling -->
          <div class="collapse navbar-collapse navbar-ex1-collapse">
            <ul class="nav navbar-nav navbar-right">
              <li class="dropdown singleDrop color-1   active ">
                <a
                  href="javascript:void(0)"
                  class="dropdown-toggle"
                  data-toggle="dropdown"
                  role="button"
                  aria-haspopup="true"
                  aria-expanded="false"
                >
                  <i class="fa fa-home bg-color-1" aria-hidden="true"></i>
                  <span class="active">Home</span>
                </a>
                <!-- <ul class="dropdown-menu dropdown-menu-left">
                  <li class=" active "><a href="/">Kidz School</a></li>
                  <li class=""><a href="index-v2.html">Kidz Store</a></li>
                  <li class=""><a href="index-v3.html">Kidz Daycare</a></li>
                  <li class=""><a href="index-v4.html">Kidz Fashion</a></li>
                </ul> -->
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
                <ul class="dropdown-menu dropdown-menu-left">
                  <li class="dropdown dropdown-submenu ">
                    <a href="javascript:void(0)" class="dropdown-toggle" data-toggle="dropdown">
                      My Categories
                      <i class="fa fa-chevron-right" aria-hidden="true"></i>
                    </a>
                    <ul class="dropdown-menu">
                      <li class=" " v-for="category in categories">
                        <!--     <a href="`/category${category.id}`">{{ category.name }}</a> -->
                        <a href="/category">{{ category.name }}</a>
                      </li>
                    </ul>
                  </li>
                </ul>
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
                  <i class="fa fa-list-ul bg-color-2" aria-hidden="true"></i>
                  <span>Tasks</span>
                </a>
                <ul class="dropdown-menu dropdown-menu-left">
                  <li class=" "><a href="/tasks">My Tasks</a></li>
                </ul>
              </li>

              <li class="dropdown singleDrop color-4 ">
                <a
                  href="javascript:void(0)"
                  class="dropdown-toggle"
                  data-toggle="dropdown"
                  role="button"
                  aria-haspopup="true"
                  aria-expanded="false"
                >
                  <i class="fa fa-pencil-square-o bg-color-4" aria-hidden="true"></i>
                  <span>Calendar</span>
                </a>
                <ul class="dropdown-menu dropdown-menu-right">
                  <li
                    class="dropdown dropdown-submenu  
                     || 
                     || 
                     "
                  ></li>
                  <li class=""><a href="/calendar">My Calendar</a></li>
                  <!--    <li class=""><a href="category.html">Product Categories</a></li>
                  <li class=""><a href="cart-page.html">Cart</a></li>
                  <li class="dropdown dropdown-submenu">
                    <a href="javascript:void(0)" class="dropdown-toggle" data-toggle="dropdown">
                      Checkout
                      <i class="fa fa-chevron-right" aria-hidden="true"></i>
                    </a>
                    <ul class="dropdown-menu">
                      <li class=" "><a href="checkout-step-1.html">Personal Info</a></li>
                      <li class=" "><a href="checkout-step-2.html">Payment Info</a></li>
                      <li class=" "><a href="checkout-step-3.html">Confirmation</a></li>
                    </ul>
                  </li> -->
                </ul>
              </li>
            </ul>
          </div>

          <div class="cart-dropdown">
            <a href="#" class="bg-color-6 shop-cart">
              <i class="fa fa-shopping-basket " aria-hidden="true"></i>
              <span class="badge bg-color-1">3</span>
            </a>
            <ul class="dropdown-menu dropdown-menu-right">
              <li>
                <i class="fa fa-shopping-basket " aria-hidden="true"></i>
                3 items in your cart
              </li>
              <li>
                <a href="single-product.html">
                  <div class="media">
                    <div class="media-left">
                      <img src="img/home/cart/cart-img.png" alt="cart-Image" />
                    </div>
                    <div class="media-body">
                      <h4>Barbie Racing Car</h4>
                      <div class="price">
                        <span class="color-1">$50</span>
                      </div>
                      <span class="amount">Qnt: 1</span>
                    </div>
                  </div>
                </a>
                <span class="cancel"><i class="fa fa-close" aria-hidden="true"></i></span>
              </li>
              <li>
                <a href="single-product.html">
                  <div class="media">
                    <div class="media-left">
                      <img src="img/home/cart/cart-img.png" alt="cart-Image" />
                    </div>
                    <div class="media-body">
                      <h4>Barbie Racing Car</h4>
                      <div class="price">
                        <span class="color-1">$50</span>
                      </div>
                      <span class="amount">Qnt: 1</span>
                    </div>
                  </div>
                </a>
                <span class="cancel"><i class="fa fa-close" aria-hidden="true"></i></span>
              </li>
              <li>
                <a href="single-product.html">
                  <div class="media">
                    <div class="media-left">
                      <img src="img/home/cart/cart-img.png" alt="cart-Image" />
                    </div>
                    <div class="media-body">
                      <h4>Barbie Racing Car</h4>
                      <div class="price">
                        <span class="color-1">$50</span>
                      </div>
                      <span class="amount">Qnt: 1</span>
                    </div>
                  </div>
                </a>
                <span class="cancel"><i class="fa fa-close" aria-hidden="true"></i></span>
              </li>
              <li>
                <span class="cart-total">Subtotal</span>
                <span class="cart-price">$150</span>
                <div class="cart-button">
                  <button
                    type="button"
                    class="btn btn-primary"
                    onclick="if (!window.__cfRLUnblockHandlers) return false; location.href='checkout-step-1.html';"
                    data-cf-modified-ba74d78c1d7257bacd4f66ec-=""
                  >
                    Checkout
                  </button>
                  <button
                    type="button"
                    class="btn btn-primary"
                    onclick="if (!window.__cfRLUnblockHandlers) return false; location.href='cart-page.html';"
                    data-cf-modified-ba74d78c1d7257bacd4f66ec-=""
                  >
                    View Cart
                  </button>
                </div>
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
                  <a href="#" class="bg-color-1"><i class="fa fa-facebook" aria-hidden="true"></i></a>
                </li>
                <li>
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
                </li>
              </ul>
            </div>
            <div class="col-sm-7 col-sm-pull-5 col-xs-12">
              <div class="copyRightText">
                <p>
                  © 2016 Copyright Kidz School Bootstrap Template by
                  <a href="http://www.iamabdus.com/">Abdus</a>
                  .
                </p>
              </div>
            </div>
          </div>
        </div>
      </div>
    </footer>
  </div>
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
  }
};
</script>
