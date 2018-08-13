<template lang="pug">
  #app
    img(src='https://mrromo.github.io/RicardoMusic/logo-white.jpg')
    h1 RicardoMusic
    select(v-model="selectedCountry")
      option(v-for="country in countries" v-bind:value="country.value") {{ country.name }}
    spinner(v-show="loading")
    ul
      artist(v-for="artist in artists" v-bind:artist="artist" v-bind:key="artist.mbid")
</template>

<script>
import Artist from './components/Artist.vue'
import getArtists from './api'
import spinner from './components/spinner'

export default {
  name: 'app',
  data () {
    return {
      loading:true,
      artists: [],
      countries: [
        { name: 'Argentina', value: 'argentina' },
        { name: 'Colombia', value: 'colombia' },
        { name: 'España', value: 'spain' },
      ],
      selectedCountry: 'argentina'
    }
  },
  components: {
    Artist,
    spinner
  },
  methods: {
    refreshArtists() {
      this.loading = true
      this.artist = []
      const self = this
      getArtists(this.selectedCountry)
        .then(function (artists) {
          self.artists = artists
          self.loading=false
        })
    }
  },
  mounted() {
    this.refreshArtists()
  },
  watch: {
    selectedCountry() {
      this.refreshArtists()
    }
  }
}
</script>

<style lang="stylus">
#app
  font-family 'Avenir', Helvetica, Arial, sans-serif
  -webkit-font-smoothing antialiased
  -moz-osx-font-smoothing grayscale
  text-align center
  color #2c3e50
  margin-top 60px

h1, h2
  font-weight normal

ul
  list-style-type none
  padding 0

li
  display inline-block
  margin 0 10px

  a
    color black
  </style>
