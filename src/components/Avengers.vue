<template>
  <v-container grid-list-md>
    <v-layout wrap>
      <v-flex xs12 md3 v-for="(avenger, index) in livingAvengers" :key="index">
        <v-card>
          <v-img
            :src="require('../assets/' + avenger.imageHref)"
            fill
            height="300"
          ></v-img>
          <v-card-title primary-title>
            <div>
              <h3 class="headline mb-0">{{avenger.name}} [{{avenger.id}}]</h3>
            </div>
          </v-card-title>
        </v-card>
      </v-flex>
    </v-layout>
  </v-container>
</template>

<script>
import axios from 'axios'

export default {
  data: () => ({
    avengers: []
  }),
  computed: {
    livingAvengers: function () {
      return this.avengers.filter(avenger => avenger.isActive)
    }
  },
  methods: {
    updateAvenger: function (payload) {
      let i = this.avengers.findIndex(avenger => avenger.id === payload.id)
      if (i < 1) {
        this.avengers.push(payload)
        return
      }
      this.avengers[i].isActive = payload.isActive
    }
  },
  created () {
    axios.get('https://localhost:8000/api/avengers')
      .then(response => {
        this.avengers = response.data['hydra:member']
        const hubUrl = response.headers.link.match(/<([^>]+)>;\s+rel=(?:mercure|"[^"]*mercure[^"]*")/)[1]
        const h = new URL(hubUrl)
        h.searchParams.append('topic', 'https://localhost:8000/api/avengers/{n}')
        const es = new EventSource(h)
        es.onmessage = e => this.updateAvenger(JSON.parse(e.data))
      })
      .catch(e => {
        console.error(e)
      })
  }
}
</script>

<style>
</style>
