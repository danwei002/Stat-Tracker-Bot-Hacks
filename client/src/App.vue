<template>
  <div class="min-vh-100" id="app">
    <Header />
    <router-view />
  </div>
</template>

<script>
import Header from "./components/Header";
export default {
  name: "app",
  components: {
    Header,
  },
};
</script>
<style>
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}
#app {
  background-image: url("./assets/necropolis.png");
  background-repeat: no-repeat;
  background-size: cover;
}
body {
  font-family: Arial, Helvetica, sans-serif;
  color: #fff;
  line-height: 1.6;
}
a {
  color: #fff;
  text-decoration: none;
}
ul {
  list-style: none;
}

.form-group {
  margin: 1rem 0;
}

.search {
  background: rgba(0, 0, 0, 0.65);
  width: 90%;
  border: 4px #cde1fa solid;
  margin-top: 1rem;
  padding: 2rem;
}

input,
select,
textarea {
  display: block;
  width: 100%;
  padding: 0.4rem;
  font-size: 1.2rem;
  border: 1px solid #ccc;
}
</style>

