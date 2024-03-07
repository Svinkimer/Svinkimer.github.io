<script>
    import * as THREE from 'three'
    import { GLTFLoader } from 'three/addons/loaders/GLTFLoader.js'
    import { OrbitControls } from 'three/addons/controls/OrbitControls.js'

    let renderer;
    let controls;
    let scene;
    let camera;
    let loader;
    let last_root;
    // let glb;


    export default {
        props: ["current_model", "mobile"],
        data() {
            return {
                name: "Mill",
                difficulty: 3,
                description: "This is amazing mill with whater wheel..."
            }
        },
        methods: {
            animate() {
                controls.update();
                renderer.render(scene, camera);
                requestAnimationFrame(this.animate);
            }
        },
        mounted() {
            // console.log(`Current model: ${this.current_model}`)
            let canvas;
            console.log(`Mobile in current model: ${this.mobile}`);
            if (this.mobile)
                canvas = document.querySelector("#mobile-canvas");
            else
                canvas = document.querySelector("#desktop-canvas");

            console.log(canvas);
            const sizes = {
                x: canvas.clientWidth,
                y: canvas.clientHeight,
            };
            // console.log(sizes);

            renderer = new THREE.WebGLRenderer({ antialias: true, canvas });
            scene = new THREE.Scene();
            camera = new THREE.PerspectiveCamera(75, sizes.x / sizes.y, 0.1, 100);
            controls = new OrbitControls(camera, canvas);
            loader = new GLTFLoader();
            camera.position.set(5, 15, 15);
            camera.lookAt(0, 0, 0);
            renderer.setSize(sizes.x, sizes.y);
            // scene.add(new THREE.AxesHelper(30));


            // const light = new THREE.AmbientLight("white", 1);
            const light = new THREE.HemisphereLight("white", "brown", 5);
            scene.add(light);

            console.log(scene);
            console.log(renderer);
            
            renderer.render(scene, camera);

            loader.load(
                    this.current_model.glb,
                    function (glb) {
                        console.log(`New glb: ${glb}`);
                        scene.add(glb.scene);
                        last_root = glb.scene;
                        // adfas
                    },
                    function (xhr) {
                        console.log(`Loaded: ${xhr.loaded/xhr.total}%`);
                    },
                    function (error) {
                        console.error(error);
                    }
                )

            // console.log(THREE)
            // console.log(GLTFLoader);
            requestAnimationFrame(this.animate);
        },
        watch: {
            current_model(model_v) {
                console.log(model_v.name);
                loader.load(
                    model_v.glb,
                    function (glb) {
                        console.log(`New glb: ${glb}`);
                        scene.remove(last_root);
                        scene.add(glb.scene);
                        last_root = glb.scene;
                    },
                    function (xhr) {
                        console.log(`Loaded: ${xhr.loaded/xhr.total}%`);
                    },
                    function (error) {
                        console.error(error);
                    }
                )
            }
        }
    }
</script>

<template>
    <div id="mobile-sections" v-if="mobile">
        <h1>{{ current_model.name}}</h1>
        <canvas id="mobile-canvas"></canvas>
        <div id="difficulty">
            <h2>Сложность:</h2>
            <div id="difficuly-marks">
                <img src="../assets/difficulty_mark.png" alt="" v-for="index in current_model.difficulty" v-bind:key="index" class="marker">  
            </div>
        </div>
        <p v-html="current_model.description"></p>
    </div>

    <div id="sections" v-else>
        <canvas id="desktop-canvas"></canvas>
        <div id="description">
            <h1>{{ current_model.name }}</h1>
            <div id="difficulty">
                <h2>Сложность:</h2>
                <div id="difficuly-marks">
                    <img src="../assets/difficulty_mark.png" alt="" v-for="index in current_model.difficulty" v-bind:key="index" class="marker">  
                    <!-- фафыв -->
                </div>
            </div>
            <p v-html="current_model.description"></p>
        </div>
    </div>
</template>

<style scoped>
    #mobile-sections {
        height: 100%;
        display: flex;
        flex-direction: column;
        justify-content: space-between;
    }
    #mobile-sections canvas {
        width: 100%;
        height: 40%;
        background-color: rgb(206, 229, 249);
    }

    #mobile-sections h1 {
        width: 100%;
        font-size: 1.7rem;
        text-align: justify;
        margin-top: 0px;
    }
    #mobile-sections #difficulty {
        height: 3vh;
        margin-top: 2vh;
    }
    #mobile-sections h2 {
        font-size: 1.2rem;
        text-align: justify;
    }
    #mobile-sections .marker {
        width: 5vw;
    }

    #mobile-sections p {
        height: 45%;
        overflow-y: scroll;
    }

    #mobile-sections #difficulty .marker {
        width: 1.5rem;
        margin-left: 1rem;
    }
</style>

<style>
    #sections {
        height: 100%;
        display: flex;
        justify-content: space-between;
    }
    #sections canvas {
        height: 100%;
        width: 50%;
        background-color: rgb(206, 229, 249);
    }

    #sections p {
        overflow-y: auto;
        height: 65%;
    }

    #description {
        display: flex;
        flex-direction: column;
        justify-content: space-between;
        /* align-items: end; */
        width: 45%;
    }

    #description h1 {
        font-size: 4rem;
        margin-top: 0px;
        margin-bottom: 0px;
    }
    #description p {
        text-align: justify;
        font-size: 1.5rem;
        margin: 0px;
    }

    #difficulty {
        height: 10%;
        display: flex;
        justify-content: space-between;
        align-items: center;
        /* margin-bottom: 1rem; */
    }
    #difficulty h2 {
        font-size: 2rem;
        margin: 0px;
    }
    #difficulty-marks{
        display: flex;
    }
    #sections #difficulty .marker {
        width: 2.5rem;
        margin-left: 1rem;
    }
</style>