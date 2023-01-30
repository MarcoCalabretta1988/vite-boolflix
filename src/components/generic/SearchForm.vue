<script>
export default {
    name: 'SearchForm',
    data: () => ({ searchTerm: '' }),
    emits: ['term-change', 'form-submit'],
    props: {
        placeholder: String,
        type: String,
        btnText: String

    }

}
</script>

<template>
    <!-- INPUT FORM -->
    <form @submit.prevent="$emit('form-submit')">

        <div class="input-group">
            <input :type="type" class="form-control" :placeholder="placeholder || 'Cerca'" v-model.trim="searchTerm"
                @keyup="$emit('term-change', searchTerm)">
            <button class="btn btn-outline-danger" type="submit" id="button-addon2">{{ btnText }}</button>
        </div>
    </form>
</template>

<style scoped lang="scss">
input {
    width: 100px;
}
</style>