<script lang="ts">
    import { onDestroy, onMount } from 'svelte';
    import * as THREE from 'three';
    import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls.js';
    import { Sky } from 'three/examples/jsm/objects/Sky.js';
    import { GLTFLoader } from 'three/examples/jsm/loaders/GLTFLoader.js';
    import GeneralStore from './GeneralStore.svelte';
    import MessageBoard from './MessageBoard.svelte';
    import Farm from './Farm.svelte';

    let container: HTMLDivElement;
    let scene: THREE.Scene;
    let camera: THREE.PerspectiveCamera;
    let renderer: THREE.WebGLRenderer;
    let controls: OrbitControls;
    let sky: Sky, sun: THREE.Vector3;
    let directionalLight: THREE.DirectionalLight;
    let spheres:any = [];
    let myModel:any;

    let showGeneralStore = false;
    let showMessageBoard = false;
    let showFarm = false;

    const raycaster = new THREE.Raycaster();
    const mouse = new THREE.Vector2();

    onMount(() => {
        init();
        window.addEventListener('click', onCanvasClick);
        window.addEventListener('resize', onWindowResize);
    });

    function init(){
        camera = new THREE.PerspectiveCamera(60, window.innerWidth / window.innerHeight, 100, 2000000);
        camera.position.set(0, 30, 450);

        scene = new THREE.Scene();

        renderer = new THREE.WebGLRenderer();
        renderer.setPixelRatio(window.devicePixelRatio);
        renderer.setSize(window.innerWidth, window.innerHeight);
        renderer.toneMapping = THREE.ACESFilmicToneMapping;
        renderer.toneMappingExposure = 0.5;
        renderer.shadowMap.enabled = true;
        renderer.shadowMap.type = THREE.PCFSoftShadowMap;
        container.appendChild(renderer.domElement);
        
        controls = new OrbitControls(camera, renderer.domElement);
        controls.maxPolarAngle = Math.PI / 2;
        controls.enableZoom = true;
        controls.enablePan = false;
        controls.minDistance = 250;
        controls.maxDistance = 500;

        initSky();
        setupDirectionalLight();
        loadModel();
        createSpheres();
        startAnimationLoop();
    }
 

    function initSky() {
        sky = new Sky();
        sky.scale.setScalar(450000);
        scene.add(sky);

        sun = new THREE.Vector3();
        const phi = THREE.MathUtils.degToRad(90 - 120);
        const theta = THREE.MathUtils.degToRad(180);
        sun.setFromSphericalCoords(1, phi, theta);

        sky.material.uniforms['sunPosition'].value.copy(sun);
    }

    function setupDirectionalLight() {
        directionalLight = new THREE.DirectionalLight(0xffffff, 4);
        directionalLight.castShadow = true;
        directionalLight.shadow.mapSize.width = 512;
        directionalLight.shadow.mapSize.height = 512;
        directionalLight.position.set(sun.x, sun.y, sun.z);
        scene.add(directionalLight);
    }

    function loadModel() {
    const loader = new GLTFLoader();
    loader.load('src/lib/scene.gltf', (gltf) => {
        myModel = gltf.scene; // Assign to global variable
        myModel.scale.set(500, 500, 500);
        myModel.castShadow = true;
        myModel.position.set(200, -30, -350);
        scene.add(myModel);
    }, undefined, (error) => {
        console.error('An error happened while loading the model:', error);
    });
    }


    function createSpheres() {
        const sphereGeometry = new THREE.SphereGeometry(5, 32, 32);
        const sphereMaterial = new THREE.MeshStandardMaterial({ 
            color: 0xe06227,
            roughness: 0.5,
            metalness: 0.1 
        });
        const positions = [
            { x: 0, y: 7, z: 0 },
            { x: 30, y: 7, z: 30 },
            { x: -30, y: 7, z: -30 }
        ];
        positions.forEach((position) => {
            const sphere = new THREE.Mesh(sphereGeometry, sphereMaterial);
            sphere.castShadow = true;
            sphere.position.set(position.x, position.y, position.z);
            spheres.push(sphere);
            scene.add(sphere);
        });
    }

    function startAnimationLoop() {
    const animate = () => {
        requestAnimationFrame(animate);

        if (myModel) {
            myModel.rotation.y += 0.001; // This value might be too high for smooth animation
        }

        controls.update();
        renderer.render(scene, camera);
    };
    animate();
    }


    function onCanvasClick(event:any) {
        mouse.x = (event.clientX / window.innerWidth) * 2 - 1;
        mouse.y = -(event.clientY / window.innerHeight) * 2 + 1;

        raycaster.setFromCamera(mouse, camera);
        const intersects = raycaster.intersectObjects(spheres);

        if (intersects.length > 0) {
            handleSphereClick(intersects[0].object);
        }
    }

    function handleSphereClick(sphere:any) {
        if (sphere === spheres[0]) {
            showGeneralStore = true;
            showMessageBoard = false;
            showFarm = false;
        } else if (sphere === spheres[1]) {
            showGeneralStore = false;
            showMessageBoard = true;
            showFarm = false;
        } else if (sphere === spheres[2]) {
            showGeneralStore = false;
            showMessageBoard = false;
            showFarm = true;
        }
    }

    function onWindowResize() {
        camera.aspect = window.innerWidth / window.innerHeight;
        camera.updateProjectionMatrix();
        renderer.setSize(window.innerWidth, window.innerHeight);
        renderer.render(scene, camera);
    }

    onDestroy(() => {
        window.removeEventListener('click', onCanvasClick);
        window.removeEventListener('resize', onWindowResize);
    });
</script>

<div bind:this={container}></div>

{#if showGeneralStore}
   <GeneralStore />
{/if}
{#if showMessageBoard}
   <MessageBoard />
{/if}
{#if showFarm}
    <Farm />
{/if}
