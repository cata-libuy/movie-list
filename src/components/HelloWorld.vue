<template>
  <div class="">
    <h1>Películas Favoritas de Cata</h1>
    <div class="movie-grid">
      <div class="movie-card" v-for="movie in orderedMovies" :key="movie.title" v-bind:style="{ backgroundImage: getBackgroundImage(movie) }">
        <div class="details">
          <h3> {{ movie.title }} </h3>
          <p> {{ movie.director }} - {{ movie.year }} </p>
          <div class="score-container">
            <a class="score score-down" v-on:click="downScore(movie)">-</a>
            <a class="score score-up" v-on:click="upScore(movie)">+</a>
          </div>
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
      return movie.img ? `url(${movie.img})` : 'url(http://linda-hoang.com/wp-content/uploads/2014/11/img-placeholder-dark.jpg)' 
    },
    upScore (movie) {
      movie.score++
    },
    downScore (movie) {
      movie.score--
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
    width: 95%;
    margin: 20px auto;
    justify-content: center;
    .movie-card {
      color: white;
      width: 25%;
      height: auto;
      padding: 10px;
      background-size: cover;
      background-position: center;
      display: flex;
      justify-content: center;
      align-items: center;
      text-align: center;
      cursor: default;
      .details {
        opacity: 0;
        transition: opacity 1s;
        h3 {
          background-color: black;
          font-size: 1.6rem;
          padding: 2px 5px;
          max-width: 80%;
          margin: auto;
        }
        p {
          background-color: black;
          font-size: 0.9rem;
          padding: 2px 5px;
          max-width: 90%;
          margin: 10px auto;
        }
        .score-container {
          display: flex;
          width: 100%;
          margin: 10px auto;
          align-items: center;
          justify-content: center;
          text-align: center;
          flex-direction: row;
          .score {
            margin: 10px;
            border-radius: 50%;
            background-color: black;
            cursor: pointer;
            width: 40px;
            height: 40px;
            display: flex;
            align-items: center;
            justify-content: center;
            text-align: center;
            font-size: 1.8rem;
            font-weight: bold;
            &.score-up {
              background-color: green;
            }
            &.score-down {
              background-color: red;
            }
          }
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
