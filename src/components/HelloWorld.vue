<template>
  <div class="">
    <h1>Películas favoritas de Catalina</h1>
    <div class="movie-grid">
      <div class="movie-card" v-for="movie in movies" :key="movie.title" v-bind:style="{ backgroundImage: getBackgroundImage(movie) }">
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
  .movie-grid {
    display: flex;
    flex-wrap: wrap;
    width: 90%;
    margin: 20px auto;
    .movie-card {
      color: white;
      width: 25%;
      height: 20vw;
      background-size: cover;
      background-position: center;
      .details {
        opacity: 0;
        transition: opacity 1s;
      }
      &:hover {
        .details {
          opacity: 1;
        }
      }
    }
  }
</style>
