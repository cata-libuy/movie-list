<template>
  <div class="movie-app">
    <h1>Películas Favoritas de Cata</h1>
    <div class="movie-grid">
      <movie-card v-for="movie in orderedMovies" :key="movie.title" :movie="movie"></movie-card>
    </div>
    <div class="fixed-btn">
      <a v-on:click="getJSON()" type="button" name="button">Imprimir JSON en consola</a>
    </div>
  </div>
</template>

<script>
import Vue from 'vue'
import VueResource from 'vue-resource'
import * as _ from 'underscore'
import MovieCard from './MovieCard'

Vue.use(VueResource)

export default {
  name: 'HelloWorld',
  components: {
    MovieCard
  },
  data: function () {
    return {
      movies: []
    }
  },
  computed: {
    orderedMovies  () {
      return _.sortBy(this.movies, (movie) => -movie.score)
    }
  },
  methods: {
    getJSON () {
      const jsonData = JSON.stringify(this.movies, undefined, 2)
      console.log(jsonData)
    }
  },
  created: function () {
    this.$http.get('data/pelis.json')
    .then(
      (res) => this.movies = res.body ? res.body : [],
      (err) => console.log(err)
    )
  }
}
</script>

<style lang="scss">
body {
  background-color: black;
  color: white;
  h1 {
    color: yellow;
    margin: 20px auto;
  }
}
.movie-app {
  .movie-grid {
    display: flex;
    flex-wrap: wrap;
    width: 95%;
    margin: 20px auto;
    justify-content: center;
  }
  .fixed-btn {
    position: fixed;
    right: 50px;
    @media (min-width: 768px) {
      top: 30px;
    }
    @media (max-width: 768px) {
      bottom: 30px;
    }
    a {
      background-color: yellow;
      color: black;
      padding: 4px 7px;
      font-size: 0.8rem;
      cursor: pointer;
      &:hover {
        background-color: red;
      }
    }
  }
}
</style>
