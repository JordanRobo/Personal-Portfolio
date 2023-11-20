<script lang="ts">
    import { onMount } from 'svelte';
    import * as THREE from 'three';
    import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls.js';
    import { Sky } from 'three/examples/jsm/objects/Sky.js';

    let container: HTMLDivElement;
    let sky: Sky, sun: THREE.Vector3, cylinder: THREE.Mesh;
    let camera: THREE.PerspectiveCamera;
    let directionalLight: THREE.DirectionalLight;

    onMount(() => {
        const scene = new THREE.Scene();
        camera = new THREE.PerspectiveCamera(60, window.innerWidth / window.innerHeight, 100, 2000000);
        camera.position.set(0, 30, 160);

        const renderer = new THREE.WebGLRenderer();
        renderer.setPixelRatio(window.devicePixelRatio);
        renderer.setSize(window.innerWidth, window.innerHeight);
        renderer.toneMapping = THREE.ACESFilmicToneMapping;
        renderer.toneMappingExposure = 0.5;
        renderer.shadowMap.enabled = true;
        renderer.shadowMap.type = THREE.PCFSoftShadowMap;
        container.appendChild(renderer.domElement);

        // Sky and Sun setup
        initSky();

        // Directional Light setup
        directionalLight = new THREE.DirectionalLight(0xffffff, 1);
        directionalLight.castShadow = true;
        directionalLight.shadow.mapSize.width = 512;
        directionalLight.shadow.mapSize.height = 512;
        scene.add(directionalLight);

        // Update directional light to match sun position
        updateDirectionalLight();

        // Cylinder geometry
        const geometry = new THREE.CylinderGeometry(60, 60, 5, 32);
        const material = new THREE.MeshStandardMaterial({ 
            color: 0x93e9be,
            roughness: 0.5,
            metalness: 0.1 
        });
        cylinder = new THREE.Mesh(geometry, material);
        cylinder.castShadow = true;
        cylinder.position.set(0, 0, 0);
        scene.add(cylinder);

        const controls = new OrbitControls(camera, renderer.domElement);
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

        function initSky() {
            sky = new Sky();
            sky.scale.setScalar(450000);
            scene.add(sky);

            sun = new THREE.Vector3();

            const phi = THREE.MathUtils.degToRad(90 - 25);
            const theta = THREE.MathUtils.degToRad(180);
            sun.setFromSphericalCoords(1, phi, theta);

            sky.material.uniforms['sunPosition'].value.copy(sun);
        }

        function updateDirectionalLight() {
            directionalLight.position.set(sun.x, sun.y, sun.z);
        }

        function onWindowResize() {
            camera.aspect = window.innerWidth / window.innerHeight;
            camera.updateProjectionMatrix();
            renderer.setSize(window.innerWidth, window.innerHeight);
            renderer.render(scene, camera);
        }
    });
</script>

<div bind:this={container}></div>
