<script>
import SearchForm from './generic/SearchForm.vue';
import SelectForm from './generic/SelectForm.vue';
import { store } from '../data/store';
export default {
    name: 'AppHeader',
    emits: ['term-change', 'form-submit'],
    data: () => ({ store }),
    components: { SearchForm, SelectForm },
}
</script>

<template>
    <header>
        <div class="container d-flex align-items-center justify-content-between">

            <div id="logo">BOOLFLIX</div>

            <div class="search-bar d-flex align-items-center">
                <select-form @choise-change="$emit('choise-change', $event)" :options-list="store.generes"
                    default-text="Tutti" class="mx-2 w-50"></select-form>
                <search-form @term-change="$emit('term-change', $event)" @form-submit="$emit('form-submit')"
                    placeholder="Cerca Film o Serie" type="text" btn-text="Cerca" class="flex-grow-1"></search-form>
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