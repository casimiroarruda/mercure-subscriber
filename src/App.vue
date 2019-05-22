<template>
  <v-app>
    <v-toolbar app>
      <v-toolbar-title class="headline text-uppercase">
        <span>Vingadores</span>
        <span class="font-weight-light"> Avante</span>
      </v-toolbar-title>
      <v-spacer></v-spacer>
      <v-btn
        flat
        href="https://github.com/duodraco/mercure-subscriber"
        target="_blank"
      >
        <v-icon color="#369">launch</v-icon> github
      </v-btn>
    </v-toolbar>

    <v-content>
      <Avengers/>
    </v-content>
  </v-app>
</template>

<script>
import Avengers from './components/Avengers'

export default {
  name: 'App',
  components: {
    Avengers
  },
  data () {
    return {
      //
    }
  }
}
</script>
