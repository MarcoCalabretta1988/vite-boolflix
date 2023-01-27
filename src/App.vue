<script>
import axios from 'axios';
import { store } from './data/store'
import { apiUri, api_key } from './data'

import Loader from './components/generic/Loader.vue';
import AppHeader from './components/AppHeader.vue';
import AppMain from './components/AppMain.vue';

export default {
  name: 'Boolflix',
  data: () => ({ store, searchTerm: '', isStartSearch: false }),
  components: { AppHeader, Loader, AppMain },
  computed: {
    axiosConfig() {
      return {
        params: {
          api_key,
          query: this.searchTerm,
          language: 'it'
        }
      }
    }
  },
  methods: {
    fetchApi(endpoint, collection) {
      store.isLoading = true;
      axios.get(`${apiUri}/${endpoint}`, this.axiosConfig)
        .then(res => { store[collection] = res.data.results; })
        .catch(error => { console.log(error) })
        .then(() => { store.isLoading = false });
    },
    onTermChange(term) {
      this.searchTerm = term

    },
    updateTitle() {
      this.isStartSearch = true
      this.fetchApi('search/movie', 'movies')
      this.fetchApi('search/tv', 'tvSeries')
    }

  }

}
</script>

<template>

  <loader v-if="store.isLoading"></loader>

  <div v-else>
    <app-header @term-change="onTermChange" @form-submit="updateTitle"></app-header>
    <app-main :search-term="searchTerm" :is-search="isStartSearch"></app-main>
  </div>

</template>

<style lang="scss">
@use './assets/scss/style.scss'
</style>