<script>
import { store } from '../data/store'

export default {
    name: 'AppMain',
    data() {
        return { store }
    },
    props: {
        searchTerm: String
    },
    methods: {
        buildLocalImagePatch(data) {
            if (!data) data = 'not'
            const url = new URL(`../assets/img/${data}.png`, import.meta.url)
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

    }
}
</script>

<template>
    <h1 class="text-center my-5 ">Risultati per : {{ searchTerm }}</h1>

    <div class="container m-5 bg-dark p-5">
        <!-- LISTA FILM -->
        <h1 class="text-center text-white">FILM:</h1>
        <ul class="list-group my-3 text-center" v-for="movie in store.movies" @key="movies.id">
            <li class="list-group-item">
                <img v-if="movie.poster_path" :src="`https://image.tmdb.org/t/p/w342${movie.poster_path}`"
                    :alt="movie.title" class="img-fluid">
                <img v-else :src="buildLocalImagePatch('noimg')" :alt="movie.title" class="img-fluid">
            </li>
            <li class="list-group-item">Titolo: {{ movie.title }}</li>
            <li class="list-group-item">Titolo Originale: {{ movie.original_title }}</li>
            <li class="list-group-item">Lingua: <img :src="buildLocalImagePatch(movie.original_language)"
                    :alt="movie.title" class="img-fluid">
            </li>
            <li class="list-group-item" v-html="transformVoteToStar(movie.vote_average)"></li>
            <li class="list-group-item">Trama: {{ movie.overview }}</li>
        </ul>

        <!-- LISTA SERIE -->
        <h1 class="text-center text-white">SERIE:</h1>
        <ul class="list-group my-3 text-center" v-for="serie in store.tvSeries" @key="movies.id">
            <li class="list-group-item"><img :src="`https://image.tmdb.org/t/p/w342${serie.poster_path}`"
                    :alt="serie.title" class="img-fluid"></li>
            <li class="list-group-item">Titolo: {{ serie.name }}</li>
            <li class="list-group-item">Titolo Originale: {{ serie.original_title }}</li>
            <li class="list-group-item">Lingua: <img :src="buildLocalImagePatch(serie.original_language)"
                    :alt="serie.name" class="img-fluid">
            </li>
            <li class="list-group-item" v-html="transformVoteToStar(serie.vote_average)"></li>
            <li class="list-group-item">Trama: {{ serie.overview }}</li>
        </ul>

    </div>
</template>

<style scoped lang="scss">

</style>