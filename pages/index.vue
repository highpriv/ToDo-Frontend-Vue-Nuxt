<template>
  <div class="wrapper">
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
  </div>
</template>

<script>
export default {
  name: "IndexPage",
  data() {
    return {};
  },
  methods: {},
};
</script>
<style scoped>
.wrapper {
  margin: 0;
  position: absolute;
  top: 50%;
  left: 50%;
  -ms-transform: translate(-50%, -50%);
  transform: translate(-50%, -50%);
  text-align: center;
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
}

.button-section {
  margin-top: 5%;
}
</style>
