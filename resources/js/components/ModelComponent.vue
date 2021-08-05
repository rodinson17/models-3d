<template>
    <div class="container">
        <div class="row justify-content-center">
            <div class="col-md-8">
                <div class="card">
                    <div class="card-header">Example Component</div>

                    <div class="card-body">
                        I'm an example component.
                    </div>

                    <!-- <canvas class="model"></canvas> -->
                    <div id="model"></div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    import * as THREE from 'three';
    //import { Stats } from './libs/stats.js';
    //import { Stats } from 'three/examples/jsm/libs/stats.module.js';
    import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls.js';
    import { FBXLoader } from 'three/examples/jsm/loaders/FBXLoader.js';
    //import { OBJLoader } from "three/examples/jsm/loaders/OBJLoader";

    export default {
        data() {
            return {
                //modelscr: 'http://localhost:8000/assets/models/samba-dancing.fbx',
                modelscr: 'http://localhost:8000/assets/models/muro-de-prueba.FBX',
                scene: null,
                camera: null,
                renderer: null,
                stats: null,
                clock: new THREE.Clock(),
                mixer: null,
            }
        } ,
        created() { },
        mounted() {
            this.init();
            this.animate();



            /* const scene = new THREE.Scene();
      const loader = new OBJLoader();
      loader.load(
        "http://localhost:8000/assets/models/Familias_Etex_Muro.obj",
        function (gltf) {
          scene.add(gltf);
          const renderer = new THREE.WebGLRenderer();
          renderer.setSize(window.innerWidth, window.innerHeight);
          document.body.appendChild(renderer.domElement);3
        },
        undefined,
        function (error) {
          console.error(error);
        }
      );*/


        // Canvas
            /* let canvas = document.getElementById('model');
            console.log('element: ', canvas);

            // Scene
            const scene = new THREE.Scene();

            let mixer, camera, renderer;

            camera = new THREE.PerspectiveCamera( 45, window.innerWidth / window.innerHeight, 1, 2000 );
            camera.position.set( 100, 200, 300 );

            // model
            const loader = new FBXLoader();
            loader.load( 'http://localhost:8000/assets/models/samba-dancing.fbx', function ( object ) {

                scene.add( object );
                scene.background = new THREE.Color(0xffffff, 0);
                //scene.add(gltf);
                const renderer = new THREE.WebGLRenderer();
                renderer.setSize(window.innerWidth, window.innerHeight);
                renderer.render(scene, camera);
                canvas.appendChild(renderer.domElement);
            } ); */

            /* renderer = new THREE.WebGLRenderer( { antialias: true } );
            renderer.setPixelRatio( window.devicePixelRatio );
            renderer.setSize( window.innerWidth, window.innerHeight );
            renderer.shadowMap.enabled = true;

            canvas.appendChild(renderer.domElement);

            renderer.render(scene, camera) */
        },
        methods: {
            init() {
                const container = document.createElement( 'div' );
				document.body.appendChild( container );

                this.camera = new THREE.PerspectiveCamera( 45, window.innerWidth / window.innerHeight, 1, 2000 );
				this.camera.position.set( 100, 200, 300 );

				this.scene = new THREE.Scene();
				this.scene.background = new THREE.Color( 0xa0a0a0 );
				this.scene.fog = new THREE.Fog( 0xa0a0a0, 200, 1000 );

                const hemiLight = new THREE.HemisphereLight( 0xffffff, 0x444444 );
				hemiLight.position.set( 0, 200, 0 );
				this.scene.add( hemiLight );

                const dirLight = new THREE.DirectionalLight( 0xffffff );
				dirLight.position.set( 0, 200, 100 );
				dirLight.castShadow = true;
				dirLight.shadow.camera.top = 180;
				dirLight.shadow.camera.bottom = - 100;
				dirLight.shadow.camera.left = - 120;
				dirLight.shadow.camera.right = 120;
				this.scene.add( dirLight );

                // ground
				const mesh = new THREE.Mesh( new THREE.PlaneGeometry( 2000, 2000 ), new THREE.MeshPhongMaterial( { color: 0x999999, depthWrite: false } ) );
				mesh.rotation.x = - Math.PI / 2;
				mesh.receiveShadow = true;
				this.scene.add( mesh );

                const grid = new THREE.GridHelper( 2000, 20, 0x000000, 0x000000 );
				grid.material.opacity = 0.2;
				grid.material.transparent = true;
				this.scene.add( grid );

                // model
				const loader = new FBXLoader();
				loader.load( this.modelscr, ( object ) => {

					this.mixer = new THREE.AnimationMixer( object );

					const action = this.mixer.clipAction( object.animations[0] );
					action.play();

					object.traverse( ( child ) => {

						if ( child.isMesh ) {
							child.castShadow = true;
							child.receiveShadow = true;
						}

					} );

					this.scene.add( object );

				} );

				this.renderer = new THREE.WebGLRenderer( { antialias: true } );
				this.renderer.setPixelRatio( window.devicePixelRatio );
				this.renderer.setSize( window.innerWidth, window.innerHeight );
				this.renderer.shadowMap.enabled = true;
				container.appendChild( this.renderer.domElement );

				const controls = new OrbitControls( this.camera, this.renderer.domElement );
				controls.target.set( 0, 100, 0 );
				controls.update();

				window.addEventListener( 'resize', this.onWindowResize() );

				// stats
				//stats = new Stats();
				//container.appendChild( stats.dom );
            },
            onWindowResize() {
                this.camera.aspect = window.innerWidth / window.innerHeight;
				this.camera.updateProjectionMatrix();

				this.renderer.setSize( window.innerWidth, window.innerHeight );
            },
            animate() {
                requestAnimationFrame( this.animate );

				const delta = this.clock.getDelta();

				if ( this.mixer ) this.mixer.update( delta );

				this.renderer.render( this.scene, this.camera );

				//this.stats.update();
            }
        }
    }
</script>
