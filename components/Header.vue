<template>
  <div class="header">
    <a class="logo">
      <v-icon color="primary" class="mb-1" @click="navbar = !navbar"
        >mdi-menu</v-icon
      >
      <span class="primary--text">ToDo</span>
    </a>
    <transition name="fade">
      <div id="mySidenav" class="sidenav" v-show="navbar">
        <a href="javascript:void(0)" class="closebtn" @click="navbar = !navbar"
          >&times;</a
        >
        <a class="sideitem" href="#">About</a>
        <a class="sideitem" href="#">Services</a>
        <a class="sideitem" href="#">Clients</a>
        <a class="sideitem" href="#">Contact</a>
      </div>
    </transition>
    <div class="header-right">
      <a class="active" href="#home">Home</a>
      <a href="#contact">Register</a>
      <a href="#about">Login</a>
    </div>
  </div>
</template>

<script>
export default {
  name: "Header",
  data() {
    return {
      navbar: false,
    };
  },
  methods: {},
};
</script>
<style scoped>
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.3s;
}
.fade-enter, .fade-leave-to /* .fade-leave-active in <2.1.8 */ {
  opacity: 0;
}
.sidenav {
  height: 100%; /* 100% Full-height */
  width: 300px; /* 0 width - change this with JavaScript */
  position: fixed; /* Stay in place */
  z-index: 1; /* Stay on top */
  top: 0; /* Stay at the top */
  left: 0;
  background-color: rgb(227, 227, 227); /* Black*/
  overflow-x: hidden; /* Disable horizontal scroll */
  padding-top: 60px; /* Place content 60px from the top */
}

/* The navigation menu links */
.sidenav a {
  padding: 8px 8px 8px 32px;
  text-decoration: none;
  font-size: 25px;
  display: inline;
  color: #121212 !important;
}

.sideitem {
  width: 100%;
}

/* When you mouse over the navigation links, change their color */
.sidenav a:hover {
  color: #eb6440 !important;
}

/* Position and style the close button (top right corner) */
.sidenav .closebtn {
  position: absolute;
  top: 0;
  right: 25px;
  font-size: 36px;
  margin-left: 50px;
}

ul {
  list-style-type: none;
}

.header {
  overflow: hidden;
  padding: 20px 10px;
}

.header a.logo {
  font-size: 25px;
  font-weight: bold;
}

.header a.active {
  color: #4fa095;
  border-bottom: 2px dashed #4fa095;
  border-radius: 0;
}

.header a:hover {
  color: #4fa095;
  border-radius: 0;
}

/* Extra small devices (phones, 600px and down) */
@media only screen and (max-width: 600px) {
  .header a {
    float: none;
    display: inline;
    text-align: left;
  }
  .header-right {
    display: none;
  }
}

/* Small devices (portrait tablets and large phones, 600px and up) */
@media only screen and (min-width: 600px) {
}

/* Medium devices (landscape tablets, 768px and up) */
@media only screen and (min-width: 768px) {
  .header a {
    float: none;
    display: inline;
    text-align: left;
  }
  .header-right {
    display: none;
  }
}

/* Large devices (laptops/desktops, 992px and up) */
@media only screen and (min-width: 992px) {
  .header-right {
    float: right;
    display: inline;
  }

  .header a {
    float: left;
    color: #bad1c2;
    text-align: center;
    padding: 12px;
    text-decoration: none;
    font-size: 18px;
    line-height: 25px;
    border-radius: 4px;
  }
}

/* Extra large devices (large laptops and desktops, 1200px and up) */
@media only screen and (min-width: 1200px) {
}
</style>
