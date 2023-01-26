<script>
import axios from 'axios';
import { store } from './data/store'
import { apiUri, apiKey } from './data'

import Loader from './components/generic/Loader.vue';
import AppHeader from './components/AppHeader.vue';
export default {
  name: 'Boolflix',
  data() {
    return { store }
  },
  components: { AppHeader, Loader },
  methods: {
    fetchMovies(query) {
      store.isLoading = true;

      const config = {
        params: {
          api_key: apiKey,
          query,
          language: 'it'
        }
      }
      axios.get(`${apiUri}/search/movie`, config)
        .then(res => { store.movies = res.data.results; })
        .catch(error => { console.log(error) })
        .then(() => { store.isLoading = false; });
    },
    fetchTvSeries(query) {
      store.isLoading = true;

      const config = {
        params: {
          api_key: apiKey,
          query,
          language: 'it'
        }

      }

      axios.get(`${apiUri}/search/tv`, config)
        .then(res => { store.tvSeries = res.data.results; })
        .catch(error => { console.log(error) })
        .then(() => { store.isLoading = false; });
    },
    buildLocalImagePatch(data) {
      if (!data) data = 'not'
      const url = new URL(`./assets/img/${data}.png`, import.meta.url)
      return url.href
    },
    transformVoteToStar(vote) {
      const transformVote = Math.ceil(Math.ceil(vote) / 2);
      let starVote = ``
      for (let i = 0; i < 5; i++) {
        if (i < transformVote) starVote += `&#9733;`
        else starVote += `&#9734;`
      }
      return starVote
    },
    onTermChange(term) {
      this.fetchMovies(term)
      this.fetchTvSeries(term)
    }
  }

}
</script>

<template>
  <loader v-if="!store.IsLoading"></loader>
  <app-header @term-change="onTermChange"></app-header>

  <div class="container m-5 bg-dark p-5">
    <!-- LISTA FILM -->
    <h1 class="text-center text-white">FILM:</h1>
    <ul class="list-group my-3 text-center" v-for="movie in store.movies" @key="movies.id">
      <li class="list-group-item">
        <img v-if="movie.poster_path" :src="`https://image.tmdb.org/t/p/w342${movie.poster_path}`" :alt="movie.title"
          class="img-fluid">
        <img v-else :src="buildLocalImagePatch('noimg')" :alt="movie.title" class="img-fluid">
      </li>
      <li class="list-group-item">Titolo: {{ movie.title }}</li>
      <li class="list-group-item">Titolo Originale: {{ movie.original_title }}</li>
      <li class="list-group-item">Lingua: <img :src="buildLocalImagePatch(movie.original_language)" :alt="movie.title"
          class="img-fluid">
      </li>
      <li class="list-group-item" v-html="transformVoteToStar(movie.vote_average)"></li>
      <li class="list-group-item">Trama: {{ movie.overview }}</li>
    </ul>

    <!-- LISTA SERIE -->
    <h1 class="text-center text-white">SERIE:</h1>
    <ul class="list-group my-3 text-center" v-for="serie in store.tvSeries" @key="movies.id">
      <li class="list-group-item"><img :src="`https://image.tmdb.org/t/p/w342${serie.poster_path}`" :alt="serie.title"
          class="img-fluid"></li>
      <li class="list-group-item">Titolo: {{ serie.name }}</li>
      <li class="list-group-item">Titolo Originale: {{ serie.original_title }}</li>
      <li class="list-group-item">Lingua: <img :src="buildLocalImagePatch(serie.original_language)" :alt="serie.name"
          class="img-fluid">
      </li>
      <li class="list-group-item" v-html="transformVoteToStar(serie.vote_average)"></li>
      <li class="list-group-item">Trama: {{ serie.overview }}</li>
    </ul>

  </div>

</template>

<style lang="scss">
@use './assets/scss/style.scss'
</style>