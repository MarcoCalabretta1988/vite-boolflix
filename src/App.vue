<script>
import axios from 'axios';
import { store } from './data/store'
import { apiUri, api_key } from './data'

import Loader from './components/generic/Loader.vue';
import AppHeader from './components/AppHeader.vue';
import AppMain from './components/AppMain.vue';

export default {
  name: 'Boolflix',
  data: () => ({ store, searchTerm: '', isStartSearch: false, actualChoise: 0 }),
  components: { AppHeader, Loader, AppMain },
  computed: {
    axiosConfig() {
      return {
        params: {
          api_key,
          query: this.searchTerm,
          language: ''
        }
      }
    }
  },
  methods: {
    fetchApi(endpoint, collection, key) {
      store.isLoading = true;
      axios.get(`${apiUri}/${endpoint}`, this.axiosConfig)
        .then(res => { store[collection] = res.data[key]; })
        .catch(error => { console.log(error) })
        .then(() => { store.isLoading = false });
    },
    onTermChange(term) {
      this.searchTerm = term;

    },
    onChoiseChange(value) {
      this.actualChoise = value ? value : 0;
    },
    updateTitle() {
      this.isStartSearch = true;
      this.actualChoise = 0;
      this.fetchApi('search/movie', 'movies', 'results');
      this.fetchApi('search/tv', 'tvSeries', 'results');
    }

  },
  created() {
    this.fetchApi('genre/movie/list', 'generes', 'genres')
  }

}
</script>

<template>

  <loader v-if="store.isLoading"></loader>

  <div v-else>
    <app-header @term-change="onTermChange" @form-submit="updateTitle" @choise-change="onChoiseChange"></app-header>
    <app-main :search-term="searchTerm" :is-search="isStartSearch" :genre-choise="actualChoise"></app-main>
  </div>

</template>

<style lang="scss">
@use './assets/scss/style.scss'
</style>