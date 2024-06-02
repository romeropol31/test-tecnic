<template>
  <v-app>
    <v-app-bar app color="#fec23d" dark>
      <div class="d-flex align-center">
        <h1 style="color: black;">Parlem Telecom Projecte</h1>
      </div>
    </v-app-bar>

    <v-main>
      <home />
    </v-main>
  </v-app>
</template>

<script>
import Home from './components/Home.vue';

export default {
  name: 'App',

  components: {Home},

  data: () => ({
  }),
};
</script>
