<template>
  <section class="wrapper" v-if="!loading">
    <Header :loggedIn="loggedIn" />
    <span class="top-text">
      <v-icon color="white"> mdi-pound </v-icon> Always Free!</span
    >
    <h2>Best ToDo App</h2>
    <h2>For Productivity</h2>
    <div class="button-section">
      <v-btn color="#FFC148" x-large class="mr-5">
        <v-icon left> mdi-apple </v-icon> Download For iOS</v-btn
      >
      <v-btn color="#FFC148" x-large>
        <v-icon left> mdi-android </v-icon> Download For Android</v-btn
      >
    </div>
  </section>
  <section v-else class="wrapperLoading load">
    <v-progress-circular
      style="left: 50%; top: 50%"
      :width="3"
      color="red"
      indeterminate
    >
    </v-progress-circular>
    <h2 class="waitText red--text">Please wait</h2>
  </section>
</template>

<script>
export default {
  name: "IndexPage",
  data() {
    return {
      loading: true,
      loggedIn: null,
    };
  },
  async mounted() {
    try {
      const response = await fetch("http://localhost:3000/user", {
        headers: { "Content-Type": "application/json" },
        credentials: "include",
      }).then((resp) => {
        this.loading = false;
        if (resp.status === 200) this.loggedIn = true;
      });
    } catch (e) {}
  },
  methods: {},
};
</script>
<style scoped>
.wrapper {
  height: 100%;
  text-align: center;
  background-color: rgb(41, 41, 41);
}
.wrapper h2 {
  font-size: 64px;
  color: white;
  font-weight: 900;
  text-shadow: 3px 3px 0px rgb(173, 173, 173);
  text-transform: uppercase;
  letter-spacing: 2px;
}

.top-text {
  display: inline-block;
  text-transform: uppercase;
  letter-spacing: 2px;
  color: rgb(255, 255, 255);
  font-weight: 900;
  margin-top: 5%;
}

.button-section {
  margin-top: 5%;
}

.wrapperLoading {
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
