<template>
	
</template>

<script>
	import * as THREE from 'three';
	import {
		OBJLoader
	} from 'three/addons/loaders/OBJLoader.js';
	import {
		OrbitControls
	} from 'three/addons/controls/OrbitControls.js';
	export default {
		name: 'HelloWorld',
		props: {
			msg: String
		},
		created() {
			let camera, scene, renderer;
			let object;
			init();

			function init() {

				camera = new THREE.PerspectiveCamera(45, window.innerWidth / window.innerHeight, 0.1, 20);
				camera.position.z = 2.5;

				scene = new THREE.Scene();

				const ambientLight = new THREE.AmbientLight(0xffffff);
				scene.add(ambientLight);

				const pointLight = new THREE.PointLight(0xffffff, 15);
				camera.add(pointLight);
				scene.add(camera);
			}

			const manager = new THREE.LoadingManager(loadModel);

			const textureLoader = new THREE.TextureLoader(manager);
			const texture = textureLoader.load(require('@/assets/man.jpg'), render);
			texture.colorSpace = THREE.SRGBColorSpace;

			function onProgress(xhr) {
				if (xhr.lengthComputable) {
					const percentComplete = xhr.loaded / xhr.total * 100;
					console.log('model ' + percentComplete.toFixed(2) + '% downloaded');
				}
			}

			function onError() {}

			const loader = new OBJLoader(manager);
			loader.load(require('@/assets/man_obj.png'), function(obj) {
				object = obj;
			}, onProgress, onError);


			renderer = new THREE.WebGLRenderer({
				antialias: true
			});
			renderer.setPixelRatio(window.devicePixelRatio);
			renderer.setSize(window.innerWidth, window.innerHeight);
			document.body.appendChild(renderer.domElement);

			const controls = new OrbitControls(camera, renderer.domElement);
			controls.minDistance = 2;
			controls.maxDistance = 5;
			controls.addEventListener('change', render);


			function loadModel() {
				object.traverse(function(child) {
					if (child.isMesh) child.material.map = texture;
				});

				object.position.y = -0.95;
				object.scale.setScalar(0.01);
				scene.add(object);
				render();
			}

			function render() {
				renderer.render(scene, camera);
			}

		}
	}
</script>