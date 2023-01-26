<script>
import axios from 'axios';
import { store } from './data/store'
import { apiUri, apiKey } from './data'

import Loader from './components/generic/Loader.vue';
import AppHeader from './components/AppHeader.vue';
import AppMain from './components/AppMain.vue';

export default {
  name: 'Boolflix',
  data() {
    return {
      store,
      searchTerm: ''
    }
  },
  components: { AppHeader, Loader, AppMain },
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
    onTermChange(term) {
      this.searchTerm = term
      this.fetchMovies(term)
      this.fetchTvSeries(term)
    }

  }

}
</script>

<template>
  <loader v-if="store.IsLoading"></loader>
  <div v-else>
    <app-header @term-change="onTermChange"></app-header>
    <app-main :search-term="searchTerm"></app-main>
  </div>

</template>

<style lang="scss">
@use './assets/scss/style.scss'
</style>