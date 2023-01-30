<script>
import { store } from '../../data/store';
import ProductionCard from './ProductionCard.vue'
export default {
    name: 'ProductionSection',
    components: { ProductionCard },
    data: () => ({ store }),
    props: {
        genre: Number,
        collection: String,
        title: String
    }
}

</script>

<template>
    <section :id="title" class="container my-5">
        <h1 class="text-center text-white">{{ title }}</h1>
        <div class="row row-cols-2 row-cols-lg-4 g-2 g-lg-3 flex-wrap " id="film-board">
            <production-card v-for="movie in store[collection]" @key="movie.id" :item="movie"
                :genre="genre"></production-card>
        </div>
    </section>
</template>

<style lang="'scss" scoped>
</style>