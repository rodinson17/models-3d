<template>
    <div class="container">
        <div class="row justify-content-center">
            <div class="col-md-8">
                <div class="card">
                    <div class="card-header">Example Component</div>

                    <div class="card-body">
                        I'm an example component.
                    </div>}

                    <canvas class="model"></canvas>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    import * as THREE from 'three';
    import { FBXLoader } from 'three/examples/jsm/loaders/FBXLoader.js';

    export default {
        created() {
            // Canvas
            let canvas = document.querySelector('canvas.model');

            // Scene
            const scene = new THREE.Scene()

            let mixer
            // model
            const loader = new FBXLoader();
            loader.load( './assets/models/samba-dancing.fbx', function ( object ) {

                /* mixer = new THREE.AnimationMixer( object );

                const action = mixer.clipAction( object.animations[ 0 ] );
                action.play();

                object.traverse( function ( child ) {

                    if ( child.isMesh ) {

                        child.castShadow = true;
                        child.receiveShadow = true;

                    }

                } ); */

                scene.add( object );

            } );
        },
        mounted() {
            console.log('Component mounted.')
        }
    }
</script>
