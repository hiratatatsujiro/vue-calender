<template>
  <v-app>
    <v-main>
      <CalenderPage />
    </v-main>
  </v-app>
</template>

<script>
import CalenderPage from "./components/CalenderPage.vue";

export default {
  name: "App",
  components: {
    CalenderPage,
  },
};
</script>
