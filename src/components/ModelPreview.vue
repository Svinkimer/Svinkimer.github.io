<script>
    export default {
        props: ["id", "model", "mobile"],
        data() {
            return {

            }
        },
        methods: {
            previewClicked() {
                // console.log(this.id);
                this.$emit('preview-clicked', this.id);
            }
        },
        mounted() {
            // console.log(this.model);
            // console.log(this.model.picture);
        }
    }
</script>

<template>
    <div v-if="mobile" id="mobile-container" @click="previewClicked">
        <h1>{{ model.name }}</h1>
        <img :src="model.picture" alt="picture_not_found">
    </div>

    <div v-else id="container" @click="previewClicked">
        <h1>{{ model.name }}</h1>
        <img :src="model.picture" alt="picture_not_found">
    </div>
</template>

<style scoped>
    #mobile-container {
        width: 70%;
        text-align: center;
    }

    #mobile-container h1 {
        font-size: 1.2rem;
        margin: 0px;
        height: 20%;
    }

    #mobile-container img {
        width: 75%;
        height: 75%;
    }
</style>

<style scoped>
    #container {
        height: 100%;
        width: 27%;
        /* background-color: yellow; */
        text-align: center;
    }
    #container:hover {
        background-color: silver;
    }
    #container h1 {
        font-size: 2rem;
        margin-top: 0.5rem;
        margin-bottom: 0.5rem;
    }
    #container img {
        width: 80%;
    }
</style>