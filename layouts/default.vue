<template>
  <v-app>
    <v-main>
      

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
  props:{
    isHeader: {
      type: Boolean,
    }
  }
};
</script>
<style scoped>
.v-application {

}
</style>
