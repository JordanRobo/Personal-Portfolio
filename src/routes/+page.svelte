<script>
    import { onMount } from 'svelte';
    import { fade } from 'svelte/transition';
    import Preloader from '../components/Preloader.svelte';

    let isLoading = true;

    onMount(async () => {
        // Perform any initial data fetching or setup here
        // Once done, set isLoading to false
        setTimeout(() => { isLoading = false; }, 4800);
    });
</script>
  
{#if isLoading}
  <Preloader />
{:else}
  <div in:fade={{ duration: 6400 }}>
    <!-- Your main page content goes here -->
    <h1 class="flex justify-center items-center h-screen">Page Title</h1>
    <!-- Other page-specific content -->
  </div>
{/if}