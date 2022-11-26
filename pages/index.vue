<template>
  <div class="wrapper">
    <span class="top-text">
      <v-icon color="#212121 "> mdi-pound </v-icon> Always Free!</span
    >
    <h2>Best ToDo App</h2>
    <h2>For Productivity</h2>
    <div class="button-section">
      <v-btn color="#FF8765" x-large class="mr-5">
        <v-icon left> mdi-apple </v-icon> Download For iOS</v-btn
      >
      <v-btn color="#FF8765" x-large>
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
  background-color: #ffc254;
  padding: 2% 10% 2% 10%;
  display: inline-block;
  text-transform: uppercase;
  letter-spacing: 2px;
  color: rgb(33, 33, 33);
  font-weight: 900;
  margin-bottom: 5%;
  box-shadow: 3px 3px rgb(53, 53, 53);
}

.button-section {
  margin-top: 5%;
}
</style>
