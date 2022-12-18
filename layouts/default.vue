<template>
  <v-app>
    <v-main>
      <Header />

      <Nuxt />
    </v-main>
  </v-app>
</template>

<script>
export default {
  name: "DefaultLayout",
  data() {
    return {};
  },
};
</script>
<style scoped>
.v-application {
  background-image: url("../static/background.png");
  background-repeat: no-repeat;
  background-size: cover;
  background-position: center;
}
</style>
