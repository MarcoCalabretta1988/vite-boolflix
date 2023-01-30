<script>
import { store } from '../data/store'

import ProductionSection from './generic/ProductionSection.vue';

export default {
    name: 'AppMain',
    components: { ProductionSection },
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
        <div v-else-if="store.movies.length === 0 && store.tvSeries.length === 0"
            class="text-white text-center m-5 p-5">
            <h1 class="text-white text-center m-5 p-5">
                <font-awesome-icon icon="fa-solid fa-face-sad-tear" class="fa-fade fa-2xl" /> nessun
                risultato
            </h1>
        </div>
        <div v-else>

            <!-- FILM SECTION -->
            <production-section title="FILM" collection="movies"></production-section>

            <!-- SERIES SECTION -->
            <production-section title="SERIE" collection="tvSeries"></production-section>

        </div>
    </main>
</template>

<style scoped lang="scss">
main {
    min-height: calc(100vh - 120px);
}
</style>