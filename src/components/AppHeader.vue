<script>
import SearchForm from './generic/SearchForm.vue';

export default {
    name: 'AppHeader',
    emits: ['term-change', 'form-submit'],
    components: { SearchForm },
}
</script>

<template>
    <header>
        <div class="container d-flex align-items-center justify-content-between">

            <div id="logo">BOOLFLIX</div>

            <div class="search-bar">
                <search-form @term-change="$emit('term-change', $event)" @form-submit="$emit('form-submit')"
                    placeholder="Cerca Film o Serie" type="text" btn-text="Cerca"></search-form>
            </div>
        </div>
    </header>
</template>

<style scoped lang="scss">
header {
    height: 120px;
    background-color: black;

    #logo {
        color: red;
        font-size: 80px;
        font-weight: bolder;
        cursor: pointer;
    }
}
</style>