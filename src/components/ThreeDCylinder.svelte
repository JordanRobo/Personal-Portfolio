<script lang="ts">
    import { onMount } from 'svelte';
    import * as THREE from 'three';
    import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls.js';
    import { Sky } from 'three/examples/jsm/objects/Sky.js';
    // import { GLTFLoader } from 'three/examples/jsm/loaders/GLTFLoader.js'; // Uncomment this line when adding a 3D model

    let container: HTMLDivElement;
    let sky: Sky,  sun: THREE.Vector3, cylinder: THREE.Mesh;

    onMount(() => {
        const scene: THREE.Scene = new THREE.Scene();
        const camera: THREE.PerspectiveCamera = new THREE.PerspectiveCamera(60, window.innerWidth / window.innerHeight, 100, 2000000);
        camera.position.set(0, 30, 160);

        const renderer: THREE.WebGLRenderer = new THREE.WebGLRenderer();
        renderer.setPixelRatio(window.devicePixelRatio);
        renderer.setSize(window.innerWidth, window.innerHeight);
        renderer.toneMapping = THREE.ACESFilmicToneMapping;
        renderer.toneMappingExposure = 0.5;
        renderer.shadowMap.enabled = true;
        renderer.shadowMap.type = THREE.PCFSoftShadowMap;
        container.appendChild(renderer.domElement);

        // Sky and Sun setup
        initSky();

        // Directional Light setup (simulating the sun)
        const directionalLight = new THREE.DirectionalLight(0xffffff, 1);
        directionalLight.position.set(50, 50, 50);
        directionalLight.castShadow = true;
        directionalLight.shadow.mapSize.width = 512;
        directionalLight.shadow.mapSize.height = 512;
        scene.add(directionalLight);

        // Cylinder geometry
        const geometry: THREE.CylinderGeometry = new THREE.CylinderGeometry(60, 60, 5, 32);
        const material: THREE.MeshStandardMaterial = new THREE.MeshStandardMaterial({ 
            color: 0x93e9be,
            roughness: 0.5, // Adjust roughness
            metalness: 0.1  // Adjust metalness
         });
        cylinder = new THREE.Mesh(geometry, material);
        cylinder.castShadow = true;
        cylinder.position.set(0, 0, 0);
        scene.add(cylinder);

        const controls: OrbitControls = new OrbitControls(camera, renderer.domElement);
        controls.maxPolarAngle = Math.PI / 2;
        controls.enableZoom = true;
        controls.enablePan = false;
        controls.minDistance = 160;
        controls.maxDistance = 300;

        window.addEventListener('resize', onWindowResize);

        const animate = (): void => {
            requestAnimationFrame(animate);
            controls.update();
            renderer.render(scene, camera);
        };
        animate();

        function initSky(): void {
            sky = new Sky();
            sky.scale.setScalar(450000);
            scene.add(sky);

            sun = new THREE.Vector3();

            // Update the sun light
            const phi: number = THREE.MathUtils.degToRad(90 - 1); // Elevation angle
            const theta: number = THREE.MathUtils.degToRad(180); // Azimuth angle
            sun.setFromSphericalCoords(1, phi, theta);

            sky.material.uniforms['sunPosition'].value.copy(sun);

        }

        function onWindowResize(): void {
            camera.aspect = window.innerWidth / window.innerHeight;
            camera.updateProjectionMatrix();
            renderer.setSize(window.innerWidth, window.innerHeight);
            renderer.render(scene, camera);
        }

        // Uncomment this section to load a 3D model
        /*
        const loader: GLTFLoader = new GLTFLoader();
        loader.load('path/to/your/model.glb', (gltf) => {
            const model: THREE.Object3D = gltf.scene;
            model.position.set(0, 0, 0); // Adjust position as needed
            model.scale.set(1, 1, 1); // Adjust scale as needed
            scene.add(model);
        });
        */
    });
</script>

<div bind:this={container}></div>
