<script lang="ts">
    import { onMount } from 'svelte';
    import { fade } from 'svelte/transition';
    import Preloader from '../components/Preloader.svelte';
    import ThreeDCylinder from '../components/ThreeDCylinder.svelte';

    let isLoading = true;

    onMount(async () => {
        // Simulated loading
        setTimeout(() => { isLoading = false; }, 4800);

        // Wait for preloader to almost finish fading out before showing main content
        setTimeout(() => {
            // Logic to show the main content can be added here if needed
        }, 2200); // Start just before the preloader finishes
    });
</script>
  
{#if isLoading}
  <Preloader />
{:else}
<div in:fade={{ duration: 3000 }}>
  <ThreeDCylinder />
  <!-- Other main content -->
</div>
{/if}