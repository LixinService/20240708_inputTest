<template>
  <v-app>
    <!-- <AppBar></AppBar> -->
    <v-main>
      <v-container>
        <v-card>
          <TestHome2 />
        </v-card>
      </v-container>

      <v-container>
        <v-card>
          <TestHome3 />
        </v-card>
      </v-container>
    </v-main>
  </v-app>
</template>

<script setup>
import TestHome2 from "./components/TestHome2.vue";
import TestHome3 from "./components/TestHome3.vue";
import AppBar from "./components/AppBar.vue";
</script>
