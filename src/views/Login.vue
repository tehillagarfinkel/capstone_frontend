<template>
  <div class="login">
    <div class="container">
      <form v-on:submit.prevent="submit()">
        <h1>Login</h1>
        <ul>
          <li class="text-danger" v-for="error in errors">{{ error }}</li>
        </ul>

        <div class="form-group">
          <i class="fa fa-envelope" aria-hidden="true"></i>
          <input
            type="email"
            class="form-control border-color-2"
            v-model="email"
            id="exampleInputEmail2"
            placeholder="Email"
          />
        </div>

        <div class="form-group">
          <i class="fa fa-lock"></i>
          <input
            type="password"
            class="form-control border-color-1"
            v-model="password"
            id="exampleInputEmail1"
            placeholder="Password"
          />
        </div>
        <input type="submit" class="btn btn-outline-primary" value="Login" />
      </form>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data: function() {
    return {
      email: "",
      password: "",
      errors: []
    };
  },
  methods: {
    submit: function() {
      var params = {
        email: this.email,
        password: this.password
      };
      axios
        .post("/api/sessions", params)
        .then(response => {
          axios.defaults.headers.common["Authorization"] = "Bearer " + response.data.jwt;
          localStorage.setItem("jwt", response.data.jwt);
          this.$router.push("/category");
        })
        .catch(error => {
          this.errors = ["Invalid email or password."];
          this.email = "";
          this.password = "";
        });
    }
  }
};
</script>
