<template>
  <div class="header">
    <a class="logo">
      <v-icon color="white" class="mb-4 burgericon" @click="navbar = !navbar"
        >mdi-menu</v-icon
      >
      <img class="headerlogo" src="../static/logo.png" />
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
body {
  background: rgb(20, 20, 20);
} /* The best color ever */
.headerlogo {
  width: 100%;
  max-width: 120px;
  height: auto;
}
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
  background-color: #5e4dd3; /* Black*/
  overflow-x: hidden; /* Disable horizontal scroll */
  padding-top: 60px; /* Place content 60px from the top */
}

/* The navigation menu links */
.sidenav a {
  padding: 8px 8px 8px 32px;
  text-decoration: none;
  font-size: 36px !important;
  margin-top: 10%;
  display: inline;
  color: #eb6440 !important;
  text-shadow: 1px 2px 0px rgb(52, 52, 52);
}

.sideitem {
  width: 100%;
}
.sidenav a:hover {
  font-size: 42px;
}
/* When you mouse over the navigation links, change their color */

/* Position and style the close button (top right corner) */
.sidenav .closebtn {
  position: absolute;
  top: 0;
  right: 25px;
  font-size: 36px;
  margin-left: 50px;
  color: #eb6440 !important;
  text-shadow: 3px 3px 0px rgb(52, 52, 52);
}

.sidenav .closebtn:hover {
  color: #eb6440 !important;
  font-size: 42px !important;
}

ul {
  list-style-type: none;
}

.header {
  overflow: hidden;
  padding: 2% 5% 2% 5%;
}

.header a.active {
  text-shadow: 3px 3px 0px rgb(52, 52, 52);
}

.header a:hover {
  border-radius: 0;
  text-shadow: 3px 3px 0px rgb(52, 52, 52);
}

.burgericon:hover {
  text-shadow: 1px 3px 0px rgb(52, 52, 52);
  margin-right: 10px;
}

/* Extra small devices (phones, 600px and down) */
@media only screen and (max-width: 600px) {
  .header a {
    float: none;
    font-size: 20px;
    display: inline;
    text-align: left;
    font-weight: 900;
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
    font-weight: 900;
    font-size: 20px;
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
    color: #ffffff;
    text-align: center;
    padding: 12px;
    text-decoration: none;
    font-size: 20px;
    line-height: 25px;
    border-radius: 4px;
    font-weight: 900;
  }
}

/* Extra large devices (large laptops and desktops, 1200px and up) */
@media only screen and (min-width: 1200px) {
}
</style>
