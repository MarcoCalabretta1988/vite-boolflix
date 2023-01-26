<script>
export default {

}

</script>

<template>

    <div class="loader">
        <div class=" loader-content text-center">
            <font-awesome-icon icon="fa-solid fa-spinner fa-spin" />
            <h1>Loading...</h1>
        </div>
    </div>
</template>

<style scoped lang="scss">
.loader {
    position: fixed;
    z-index: 10;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background-color: black;
    display: flex;
    justify-content: center;
    align-items: center;

    .loader-content {
        width: 400px;
        height: 400px;
        display: flex;
        justify-content: center;
        align-items: center;
        flex-direction: column;
        border-radius: 20px;
        border: 3px solid red;
        color: red;

    }

}
</style>