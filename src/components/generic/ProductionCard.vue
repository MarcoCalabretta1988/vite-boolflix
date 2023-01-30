<script>
import axios from 'axios';
import { apiUri, api_key } from '../../data/index'
export default {
    name: 'ProductionCard',
    data: () => ({ cast: [] }),
    props: { item: Object, genre: Number },
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
        getFiveMemberCast() {
            axios.get(`${apiUri}//movie/${this.item.id}/credits?api_key=${api_key}`)
                .then(res => {
                    const castNames = res.data.cast.map((actor) => actor.name)
                    this.cast = castNames.filter((actor, i) => i < 5 ? true : false)
                })
        }
    },
    created() {
        this.getFiveMemberCast();
    }

}
</script>

<template>
    <div v-if="item.genre_ids.includes(genre) || !genre" class="card">
        <div class="poster">

            <img v-if="item.poster_path" :src="`https://image.tmdb.org/t/p/w342${item.poster_path}`" :alt="item.title"
                class="img-fluid">
            <div v-else class="text-center noimg">
                <div class="show-name m-2">{{ item.title || item.name }}</div>
                <img src="../../assets/img/noimg.png" :alt="item.title" class="img-fluid my-3">
            </div>

        </div>

        <ul class="list-group text-center item-info">

            <li class="list-group-item">Titolo: {{ item.title || item.name }}</li>
            <li class="list-group-item">Titolo Originale: {{ item.original_title }}</li>
            <li class="list-group-item flag">
                <img v-if="hasFlag" :src="buildLocalImagePatch" :alt="item.title" class="img-fluid">
                <div v-else>{{ item.original_language }}</div>
            </li>
            <li class="list-group-item avarage" v-html="transformVoteToStar(item.vote_average)"></li>
            <li class="list-group-item " id="cast">
                <h5>CAST</h5>
                <ul class="text-center p-0">
                    <li class="d-inline" v-for="actor in cast" :key="actor"> {{ actor + ' ,'}} </li>
                </ul>
            </li>
            <li class="list-group-item overview">Trama:
                <div>
                    {{ item.overview }}
                </div>
            </li>
        </ul>
    </div>

</template>

<style lang="scss" scoped>
.card {
    position: relative;
    height: 500px;
    background-color: black;
    overflow: auto;

    img {
        height: 100%;
    }

    &:hover .poster {
        display: none;
    }

    .show-name {
        background-color: white;
        font-size: 20px;
        height: 50px;
        display: flex;
        align-items: center;
        justify-content: center;
    }
}

.poster {

    position: absolute;
    top: 0;
    left: 0;
    z-index: 1;
    height: 100%;

    .noimg {
        background-color: black;
    }
}

ul {

    height: 100%;
    font-size: 18px;

    li {
        background-color: black;
        color: white;
        list-style-type: none;

        &.flag {
            img {
                width: auto;
                height: 50px;
            }
        }

        &.overview {

            height: 100%;
            overflow: auto;
        }

        &.avarage {
            font-size: 25px;
        }
    }
}
</style>