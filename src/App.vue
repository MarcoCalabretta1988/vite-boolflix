<script>
import axios from 'axios';
import { store } from './data/store'
import { apiUri, apiKey } from './data'
export default {
  name: 'Boolflix',
  data() {
    return {
      store,
      searchTerm: ''
    }
  },
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
    buildImagePatch(data) {
      if (!data) data = 'not'
      const url = new URL(`./assets/img/${data}.png`, import.meta.url)
      console.log(url.href)
      return url.href
    }
  }

}
</script>

<template>
  <div class="container m-5 bg-dark p-5">

    <!-- INPUT DI RICERCA -->
    <div class="input-group mb-3">
      <input type="text" class="form-control" placeholder="Cerca film o serie" v-model="searchTerm"
        @keyup.enter="fetchMovies(searchTerm), fetchTvSeries(searchTerm)">
      <button class="btn btn-outline-danger" type="button" id="button-addon2"
        @click="fetchMovies(searchTerm)">Cerca</button>
    </div>

    <!-- LISTA FILM -->
    <h1 class="text-center text-white">FILM:</h1>
    <ul class="list-group my-3 text-center" v-for="movie in store.movies" @key="movies.id">
      <li class="list-group-item">Titolo: {{ movie.title }}</li>
      <li class="list-group-item">Titolo Originale: {{ movie.original_title }}</li>
      <li class="list-group-item">Lingua: <img :src="buildImagePatch(movie.original_language)" :alt="movie.title"
          class="img-fluid">
      </li>
      <li class="list-group-item">Voto: {{ movie.vote_average }}</li>
      <li class="list-group-item">Trama: {{ movie.overview }}</li>
    </ul>

    <!-- LISTA SERIE -->
    <h1 class="text-center text-white">SERIE:</h1>
    <ul class="list-group my-3 text-center" v-for="serie in store.tvSeries" @key="movies.id">
      <li class="list-group-item">Titolo: {{ serie.name }}</li>
      <li class="list-group-item">Titolo Originale: {{ serie.original_title }}</li>
      <li class="list-group-item">Lingua: <img :src="buildImagePatch(serie.original_language)" :alt="serie.name"
          class="img-fluid">
      </li>
      <li class="list-group-item">Voto: {{ serie.vote_average }}</li>
      <li class="list-group-item">Trama: {{ serie.overview }}</li>
    </ul>

  </div>

</template>

<style lang="scss">
ul {
  li {
    img {
      width: 100px;
      height: auto;
    }
  }
}
</style>