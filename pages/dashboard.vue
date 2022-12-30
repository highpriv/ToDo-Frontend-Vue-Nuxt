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
          <Board
            :selectedPage="selectedPage"
            :taskCount="loggedUser.taskCount"
          />
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
import Board from "../components/Board.vue";

export default {
  name: "dashboard",
  data() {
    return {
      loading: true,
      loggedUser: {},
      selectedItem: 0,
      tasks: {},
      items: [
        {
          text: "To Do List",
          icon: "mdi-calendar-check",
          status: "todo",
        },
        {
          text: "Ongoing Tasks",
          icon: "mdi-clipboard-text-play",
          status: "ongoing",
        },
        {
          text: "Completed Tasks",
          icon: "mdi-text-box-check",
          status: "completed",
        },

        { text: "Logout", icon: "mdi-logout" },
      ],
    };
  },

  computed: {
    selectedPage() {
      return this.items[this.selectedItem];
    },
  },
  methods: {},
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
