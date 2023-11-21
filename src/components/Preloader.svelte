<script lang="ts">
   import { fade } from 'svelte/transition';
   import { onMount } from 'svelte';

    // Define Component: ProgressBar
    let progress = 0; // Initialize State

    // Define the duration of the progress in milliseconds
    const duration = 3600; 

    onMount(() => {
    const interval = setInterval(() => {
        progress += 100 / (duration / 100); // Increment progress
        if (progress >= 100) {
        clearInterval(interval);
        progress = 100;
        }
    }, 100); // Update every 100ms
    });
</script>
  
<div in:fade={{ duration: 2400 }} out:fade={{ duration: 2400 }} class="preloader flex justify-center items-center h-screen">
    <div>
        <h1>Your Adventure is About to Begin...</h1>
        <progress class="nes-progress is-primary" value="{progress}" max="100"></progress>
    </div>
</div> 