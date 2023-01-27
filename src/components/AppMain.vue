<script>
import { store } from '../data/store'
import ProductionCard from './generic/ProductionCard.vue'

export default {
    name: 'AppMain',
    components: { ProductionCard },
    data: () => ({ store }),
    props: {
        searchTerm: String,
        isSearch: Boolean
    },
}
</script>

<template>
    <main class="bg-dark p-5">
        <div v-if="!isSearch">
            <h1 class="text-white text-center m-5 p-5">Digita cosa cercare</h1>
        </div>
        <div v-else>

            <!-- FILM SECTION -->
            <section id="film-section" class="container my-5">
                <h1 class="text-center text-white">FILM:</h1>
                <div class="row row-cols-2 row-cols-lg-4 g-2 g-lg-3 flex-wrap " id="film-board">
                    <production-card v-for="movie in store.movies" @key="movie.id" :item="movie"></production-card>
                </div>
            </section>

            <!-- SERIES SECTION -->
            <section id="tvseries-section" class="container ">
                <h1 class="text-center text-white">SERIE:</h1>
                <div class="row row-cols-2 row-cols-lg-4 g-2 g-lg-3  flex-wrap " id="tv-board">
                    <production-card v-for="serie in store.tvSeries" @key="serie.id" :item="serie"></production-card>
                </div>
            </section>
        </div>
    </main>
</template>

<style scoped lang="scss">
main {
    min-height: calc(100vh - 120px);
}
</style>