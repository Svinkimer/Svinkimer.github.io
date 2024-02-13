<script>
    export default {
        props: ["id", "model"],
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
    <div id="container" @click="previewClicked">
        <h1>{{ model.name }}</h1>
        <img :src="model.picture" alt="picture_not_found">
    </div>
</template>

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