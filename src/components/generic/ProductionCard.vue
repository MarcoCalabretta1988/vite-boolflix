<script>

export default {
    name: 'ProductionCard',
    props: { item: Object },
    computed: {
        buildLocalImagePatch() {
            const url = new URL(`../../assets/img/${this.item.original_language}.png`, import.meta.url)
            return url.href
        },
        hasFlag() {
            const flags = ['it', 'en', 'de', 'es', 'fr', 'ja', 'ko', 'ru', 'us', 'za'];
            return flags.includes(this.item.original_language);
        }

    },
    methods: {

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
    <img :src="`https://image.tmdb.org/t/p/w342${item.poster_path}`" :alt="item.title" class="img-fluid">
    <ul class="list-group text-center item-info">

        <li class="list-group-item">Titolo: {{ item.title || item.name }}</li>
        <li class="list-group-item">Titolo Originale: {{ item.original_title }}</li>
        <li class="list-group-item flag">Lingua:

            <img v-if="hasFlag" :src="buildLocalImagePatch" :alt="item.title" class="img-fluid">
            <div v-else>{{ item.original_language }}</div>
        </li>
        <li class="list-group-item" v-html="transformVoteToStar(item.vote_average)"></li>
        <li class="list-group-item overview">Trama: {{ item.overview }}</li>
    </ul>


</template>

<style lang="scss" scoped>
ul {
    height: 600px;

    li {
        background-color: black;
        color: white;


        &.flag {
            img {
                width: auto;
                height: 80px;
            }
        }

        &.overview {

            height: 100%;
            overflow: auto;
        }
    }
}
</style>