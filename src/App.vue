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
  }

}
</script>

<template>
  <div class="container m-5 bg-dark p-5">

    <div class="input-group mb-3">
      <input type="text" class="form-control" placeholder="Cerca film o serie" v-model="searchTerm"
        @keyup.enter="fetchMovies(searchTerm)">
      <button class="btn btn-outline-danger" type="button" id="button-addon2"
        @click="fetchMovies(searchTerm)">Cerca</button>
    </div>

    <ul class="list-group my-3 text-center" v-for="movie in store.movies" @key="movies.id">
      <li class="list-group-item">Titolo: {{ movie.title }}</li>
      <li class="list-group-item">Titolo Originale: {{ movie.original_title }}</li>
      <li class="list-group-item">Lingua: {{ movie.original_language }}</li>
      <li class="list-group-item">Voto: {{ movie.vote_average }}</li>
      <li class="list-group-item">Trama: {{ movie.overview }}</li>
    </ul>
  </div>
</template>

<style lang="scss">

</style>