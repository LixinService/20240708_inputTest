<template>
  <v-app>
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
</script>
