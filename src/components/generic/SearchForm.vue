<script>
export default {
    name: 'SearchForm',
    data() {
        return {
            searchTerm: ''
        }
    },
    emits: ['term-change'],
    props: {
        placeholder: String,
        type: String,
        btnText: String

    }

}
</script>

<template>
    <!-- INPUT FORM -->
    <div class="input-group">
        <input :type="type" class="form-control" :placeholder="placeholder" v-model="searchTerm"
            @keyup.enter="$emit('term-change', searchTerm)">
        <button class="btn btn-outline-danger" type="button" id="button-addon2"
            @click="$emit('term-change', searchTerm)">{{ btnText }}</button>
    </div>
</template>