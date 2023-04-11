<script setup>
import { RouterView } from 'vue-router'
</script>

<template>
   <v-app>
      <v-app-bar app color="grey-darken-4">
         <v-toolbar-title>Poster Editor</v-toolbar-title>
         <v-spacer></v-spacer>
         <v-btn color="dark" to="/">Github</v-btn>
      </v-app-bar>
      <v-main>
          <RouterView />
      </v-main>
    </v-app>
</template>

