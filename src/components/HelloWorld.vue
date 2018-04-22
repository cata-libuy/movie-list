<template>
  <div class="movie-app">
    <h1>Películas Favoritas de Cata</h1>
    <div class="movie-grid">
      <movie-card v-for="movie in orderedMovies" :key="movie.title" :movie="movie"></movie-card>
    </div>
    <div class="fixed-btn">
      <a v-on:click="getJSON()" type="button" name="button">Obtener JSON</a>
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
      const jsonData = JSON.stringify(this.movies, undefined, 2);
      alert('json en consola js')
      console.log(jsonData)
    }
  },
  created: function () {
    this.$http.get('data/pelis.json')
    .then(
      (res) => this.movies = res.body ? res.body : [],
      (err) => console.log(err)
    )
  },
  watch: {
    movies: function (val) {
      console.log(val)
    }
  }
}
</script>

<style lang="scss">
body {
  background-color: black;
  color: white;
  h1 {
    color: yellow;
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
    bottom: 30px;
    right: 50px;
    a {
      background-color: yellow;
      color: black;
      padding: 5px 10px;
    }
  }
}
</style>
