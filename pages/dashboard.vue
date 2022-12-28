<template :isHeader="headerStatus">
  <div class="wrapper" v-if="!loading">
    <v-card width="256" flat class="sidebar">
      <v-navigation-drawer permanent>
        <v-list>
          <v-list-item>
            <v-list-item-avatar>
              <v-img src="https://cdn.vuetifyjs.com/images/john.png"></v-img>
            </v-list-item-avatar>
          </v-list-item>

          <v-list-item link>
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
        <v-list nav dense>
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
  name: "dashboard",
  data() {
    return {
      loading: true,
      loggedUser: {},
      selectedItem: 0,
      items: [
        { text: "To Do List", icon: "mdi-folder" },
        { text: "Ongoing Tasks", icon: "mdi-star" },
        { text: "Completed Tasks", icon: "mdi-account-multiple" },
        { text: "My Notes", icon: "mdi-history" },
        { text: "My Agenda", icon: "mdi-check-circle" },
        { text: "Group Tasks", icon: "mdi-upload" },
        { text: "Logout", icon: "mdi-cloud-upload" },
      ],
    };
  },
  async mounted() {
    try {
      const resp = await fetch("http://localhost:3000/user", {
        headers: { "Content-Type": "application/json" },
        credentials: "include",
      });
      if (resp.status === 200)
        (this.loading = false), console.log("asdasd", await resp.json());
      else this.$router.push({ name: "index" });
    } catch (err) {
      this.$router.push({ name: "index" });
    }
  },
};
</script>
<style scoped>
.sidebar {
  margin-left: 6%;
  top: 10%;
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
