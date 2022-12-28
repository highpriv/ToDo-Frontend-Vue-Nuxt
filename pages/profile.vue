<template :isHeader="headerStatus">
  <div class="wrapper" v-if="!loading">
    <v-row>
      <v-col md="2" sm="2" class="ma-12">
        <v-card elevation="2" width="100%" flat>
          <v-navigation-drawer permanent>
            <v-list class="d-flex justify-center grey lighten-3" elevation="10">
              <v-list-item
                class="ml-0"
                link
                @click="$router.push({ name: 'profile' })"
              >
                <v-list-item-avatar>
                  <v-img
                    src="https://cdn.vuetifyjs.com/images/john.png"
                  ></v-img>
                </v-list-item-avatar>
                <v-list-item-content>
                  <v-list-item-title class="text-h6">
                    @{{ loggedUser.username }}
                  </v-list-item-title>
                  <v-list-item-subtitle>{{
                    loggedUser.email
                  }}</v-list-item-subtitle>
                </v-list-item-content>
              </v-list-item>
            </v-list>
            <v-divider></v-divider>
            <v-list nav class="mt-6">
              <v-list-item-group v-model="selectedItem" color="primary">
                <v-list-item v-for="(item, i) in items" :key="i">
                  <v-list-item-icon>
                    <v-icon v-text="item.icon"></v-icon>
                  </v-list-item-icon>

                  <v-list-item-content>
                    <v-list-item-title v-text="item.text"></v-list-item-title>
                  </v-list-item-content>
                </v-list-item>
              </v-list-item-group>
            </v-list>
          </v-navigation-drawer>
        </v-card>
      </v-col>
      <v-col class="ma-12">
        <v-card
          elevation="2"
          min-width="100%"
          min-height="100%"
          flat
          class="pa-5"
        >
          <h2 class="text--disabled grey lighten-3 pa-2">Profile Settings</h2>
          <v-form v-model="valid">
            <v-container>
              <v-row class="mt-5">
                <v-col cols="12" md="12">
                  <v-text-field
                    v-model="updateForm.name"
                    :rules="nameRules"
                    :label="loggedUser.name || 'First name'"
                    required
                  ></v-text-field>
                </v-col>

                <v-col cols="12" md="12">
                  <v-text-field
                    v-model="updateForm.surname"
                    :rules="nameRules"
                    :label="loggedUser.surname || 'Last name'"
                    required
                  ></v-text-field>
                </v-col>

                <v-col cols="12" md="12">
                  <v-text-field
                    v-model="updateForm.username"
                    :rules="nameRules"
                    :counter="12"
                    :label="'@' + loggedUser.username"
                    required
                  ></v-text-field>
                </v-col>

                <v-col cols="12" md="12">
                  <v-text-field
                    v-model="updateForm.email"
                    :rules="emailRules"
                    :label="loggedUser.email"
                    required
                  ></v-text-field>
                </v-col>

                <v-col cols="12" md="12">
                  <h2 class="text--disabled grey lighten-3 pa-2">
                    Change Password
                  </h2>
                  <v-col cols="12" md="12">
                    <v-text-field
                      v-model="updateForm.current_password"
                      :rules="nameRules"
                      :counter="10"
                      label="Current Password"
                      required
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" md="12">
                    <v-text-field
                      v-model="updateForm.password"
                      :rules="nameRules"
                      :counter="10"
                      label="New Password"
                      required
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" md="12">
                    <v-text-field
                      v-model="lastname"
                      :rules="nameRules"
                      :counter="10"
                      label="New Password (Again)"
                      required
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" md="12">
                    <v-btn
                      block
                      large
                      class="mt-5"
                      color="primary"
                      @click="updateProfile()"
                      >Save</v-btn
                    >
                  </v-col>
                </v-col>
              </v-row>
              <v-snackbar v-model="showSnack.status" timeout="2000">
                {{ showSnack.message }}

                <template v-slot:action="{ attrs }">
                  <v-btn
                    color="pink"
                    text
                    v-bind="attrs"
                    @click="showSnack.status = false"
                  >
                    Close
                  </v-btn>
                </template>
              </v-snackbar>
            </v-container>
          </v-form>
        </v-card>
      </v-col>
    </v-row>
  </div>
  <div v-else class="wrapper load">
    <v-progress-circular
      style="left: 50%; top: 50%"
      :width="3"
      color="red"
      indeterminate
    >
    </v-progress-circular>
    <h2 class="waitText red--text">Please wait</h2>
  </div>
</template>

<script>
export default {
  name: "profile",
  data() {
    return {
      loading: true,
      showSnack: {
        status: false,
        message: "",
      },
      updateForm: {
        name: "",
        surname: "",
        username: "",
        email: "",
        password: "",
        current_password: "",
      },
      loggedUser: {},
      selectedItem: 0,
      items: [
        { text: "To Do List", icon: "mdi-calendar-check" },
        { text: "Ongoing Tasks", icon: "mdi-clipboard-text-play" },
        { text: "Completed Tasks", icon: "mdi-text-box-check" },
        { text: "My Notes", icon: "mdi-clipboard-edit" },
        { text: "My Agenda", icon: "mdi-view-agenda" },
        { text: "Group Tasks", icon: "mdi-account-group" },
        { text: "Logout", icon: "mdi-logout" },
      ],
    };
  },
  methods: {
    async updateProfile() {
      try {
        await fetch("http://localhost:3000/update-profile", {
          method: "POST",
          headers: { "Content-Type": "application/json" },
          credentials: "include",
          body: JSON.stringify({
            name: this.updateForm.name || this.loggedUser.name,
            surname: this.updateForm.surname || this.loggedUser.surname,
            username: this.updateForm.username || this.loggedUser.username,
            email: this.updateForm.email || this.loggedUser.email,
            password: this.updateForm.password,
            current_pw: this.updateForm.current_password,
          }),
        }).then((resp) => {
          if (resp.status === 200)
            this.showSnack = {
              status: true,
              message: "Your profile updated successfully!",
            };
          else
            this.showSnack = {
              status: true,
              message: "An error occured!",
            };
        });
      } catch (err) {}
    },
  },
  async mounted() {
    try {
      const resp = await fetch("http://localhost:3000/user", {
        headers: { "Content-Type": "application/json" },
        credentials: "include",
      });
      if (resp.status === 200)
        (this.loading = false), (this.loggedUser = await resp.json());
      else this.$router.push({ name: "index" });
    } catch (err) {
      this.$router.push({ name: "index" });
    }
  },
};
</script>
<style scoped>
.board {
  top: 10%;
  top: 0;
  left: 0;
}
.wrapper {
  height: 100%;
  background-color: rgb(243, 243, 243);
}
.load {
  width: 100%;
  display: flex;
}
.waitText {
  width: 100%;
  display: flex;
  text-align: center;
  align-items: center;
  justify-content: center;
  margin-top: 10vw;
}
</style>
