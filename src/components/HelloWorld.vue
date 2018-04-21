<template>
  <div class="">
    <h1>Películas favoritas de Catalina</h1>
    <div class="movie-grid">
      <div class="movie-card" v-for="movie in orderedMovies" :key="movie.title" v-bind:style="{ backgroundImage: getBackgroundImage(movie) }">
        <div class="details">
          <h3> {{ movie.title }} </h3>
          <p> {{ movie.year }} </p>
          <p> {{ movie.director }} </p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Vue from 'vue'
import VueResource from 'vue-resource'
import * as _ from 'underscore'

Vue.use(VueResource)

export default {
  name: 'HelloWorld',
  data: function () {
    return {
      movies: []
    }
  },
  methods: {
    getBackgroundImage (movie) {
      return `url(${movie.img})`
    }
  },
  computed: {
    orderedMovies  () {
      return _.sortBy(this.movies, (movie) => -movie.score)
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
  .movie-grid {
    display: flex;
    flex-wrap: wrap;
    width: 90%;
    margin: 20px auto;
    justify-content: center;
    .movie-card {
      color: white;
      width: 25%;
      height: 20vw;
      background-size: cover;
      background-position: center;
      display: flex;
      justify-content: center;
      align-items: center;
      text-align: center;
      .details {
        opacity: 0;
        transition: opacity 1s;
        h3 {
          background-color: black;
          padding: 2px 5px;
          max-width: 80%;
          margin: auto;
        }
      }
      &:hover {
        .details {
          opacity: 1;
        }
      }
    }
  }
</style>
