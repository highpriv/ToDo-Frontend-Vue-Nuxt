<template>
  <v-app>
    <v-main>
      <Header />

      <Nuxt />
    </v-main>
  </v-app>
</template>

<script>
import Header from "../components/Header.vue";

export default {
  name: "DefaultLayout",
  data() {
    return {};
  },
};
</script>
<style scoped></style>
