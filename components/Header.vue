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
      <v-dialog transition="dialog-bottom-transition" max-width="600">
        <template v-slot:activator="{ on, attrs }">
          <a v-bind="attrs" v-on="on">Register</a>
        </template>
        <template>
          <v-card>
            <v-toolbar color="primary white--text">REGISTER</v-toolbar>

            <v-card-text>
              <v-form ref="form" v-model="valid" lazy-validation>
                <v-col class="mt-5">
                  <v-row>
                    <v-text-field
                      label="Username"
                      v-model="usernameregister"
                      :rules="[nameRules, required]"
                      solo
                      class="mr-2"
                    ></v-text-field>
                    <v-text-field
                      :rules="[emailRules, required]"
                      v-model="emailregister"
                      label="E-Mail"
                      solo
                    ></v-text-field>
                  </v-row>
                  <v-row>
                    <v-text-field
                      label="Password"
                      v-model="registerpw"
                      :rules="[pwRegisterRules, required]"
                      :append-icon="showRegisterPw ? 'mdi-eye' : 'mdi-eye-off'"
                      :type="showRegisterPw ? 'text' : 'password'"
                      @click:append="showRegisterPw = !showRegisterPw"
                      solo
                      class="mr-2"
                    ></v-text-field>
                    <v-text-field
                      v-model="registerpw2"
                      :append-icon="showRegisterPw2 ? 'mdi-eye' : 'mdi-eye-off'"
                      :type="showRegisterPw2 ? 'text' : 'password'"
                      :rules="[pwRegisterRules, required]"
                      @click:append="showRegisterPw2 = !showRegisterPw2"
                      label="Password (Again)"
                      solo
                    ></v-text-field>
                  </v-row>
                  <v-row>
                    <v-checkbox
                      v-model="termsofuse"
                      :rules="[(v) => !!v || 'You must agree to continue!']"
                      required
                      label="I accept the therms of use *"
                    ></v-checkbox>
                  </v-row>
                  <v-row class="mt-0">
                    <v-checkbox
                      v-model="permissionbox"
                      label="I want to recieve information mails"
                    ></v-checkbox>
                  </v-row>
                </v-col>
                <v-row justify="end" class="ma-2">
                  <v-btn fluid large color="primary" @click="createNewAccount"
                    >Create Account</v-btn
                  >
                </v-row>
              </v-form>
            </v-card-text>
          </v-card>
        </template>
      </v-dialog>

      <v-dialog transition="dialog-bottom-transition" max-width="600">
        <template v-slot:activator="{ on, attrs }">
          <a v-bind="attrs" v-on="on">Login</a>
        </template>
        <template>
          <v-card>
            <v-toolbar color="primary white--text">LOGIN</v-toolbar>
            <v-form ref="form" v-model="valid" lazy-validation>
              <v-card-text>
                <v-col class="mt-5">
                  <v-text-field
                    label="Username or E-Mail"
                    solo
                    v-model="loginid"
                    class="mr-2"
                  ></v-text-field>

                  <v-text-field
                    v-model="loginpw"
                    :append-icon="showLoginPw ? 'mdi-eye' : 'mdi-eye-off'"
                    :type="showLoginPw ? 'text' : 'password'"
                    @click:append="showLoginPw = !showLoginPw"
                    label="Password"
                    solo
                    class="mr-2"
                  ></v-text-field>
                </v-col>
                <v-row justify="end" class="ma-2">
                  <v-btn fluid large color="primary" @click="login"
                    >Login</v-btn
                  >
                </v-row>
              </v-card-text>
            </v-form>
          </v-card>
        </template>
      </v-dialog>

      <v-snackbar v-model="registerSuccess" timeout="2000">
        You registered successfully, redirecting...

        <template v-slot:action="{ attrs }">
          <v-btn
            color="blue"
            text
            v-bind="attrs"
            @click="this.registerSuccess = false"
          >
            Close
          </v-btn>
        </template>
      </v-snackbar>

      <v-snackbar v-model="registerError" timeout="2000">
        An error occurred when register. Please try again.

        <template v-slot:action="{ attrs }">
          <v-btn
            color="blue"
            text
            v-bind="attrs"
            @click="this.registerError = false"
          >
            Close
          </v-btn>
        </template>
      </v-snackbar>
    </div>
  </div>
</template>

<script>
export default {
  name: "Header",
  data() {
    return {
      navbar: false,
      registerSuccess: false,
      registerError: false,
      termsofuse: false,
      permissionbox: false,
      valid: true,
      nameRules: [
        (v) => (v && v.length <= 10) || "Name must be less than 10 characters",
      ],
      pwRegisterRules: [
        (v) =>
          (v && v.length >= 9) || "Password must be greater than 10 characters",
        (v) => v == this.registerpw2 || "Passwords not same",
      ],
      required: [(v) => !!v || "This field is required"],
      emailRules: [(v) => /.+@.+\..+/.test(v) || "E-mail must be valid"],
      showLoginPw: false,
      showRegisterPw: false,
      showRegisterPw2: false,
      loginid: "",
      loginpw: "Password",
      registerpw: "Password",
      registerpw2: "Password",
      usernameregister: "",
      emailregister: "",
    };
  },
  methods: {
    async createNewAccount() {
      try {
        await this.$axios
          .post("users", {
            username: this.usernameregister,
            password: this.registerpw,
            email: this.emailregister,
          })
          .then((resp) => {
            if (resp.status === 200) this.registerSuccess = true;
            else this.registerError = true;
          });
      } catch (e) {
        this.registerError = true;
      }
    },
    async login() {
      try {
        await this.$axios
          .post("login", {
            username: this.loginid,
            password: this.loginpw,
          })
          .then((resp) => {
            if (resp.status === 200) this.registerSuccess = true;
            else this.registerError = true;
          });
      } catch (e) {
        this.registerError = true;
      }
    },
  },
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
  border-radius: 0px 50px 50px 0px;
  position: fixed; /* Stay in place */
  z-index: 1; /* Stay on top */
  top: 0; /* Stay at the top */
  left: 0;
  background-color: #292929;
  box-shadow: 5px 1px 10px rgb(34, 34, 34);
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
  color: #ffffff !important;
  text-shadow: 1px 2px 0px rgb(52, 52, 52);
}

.sideitem {
  width: 100%;
}
.sidenav a:hover {
  color: #eb6440 !important;
}
/* When you mouse over the navigation links, change their color */

/* Position and style the close button (top right corner) */
.sidenav .closebtn {
  position: absolute;
  top: 0;
  right: 25px;
  font-size: 36px;
  margin-left: 50px;
  color: #ffffff !important;
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
