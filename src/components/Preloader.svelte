<script lang="ts">
   import { onMount } from 'svelte';
   import { fade } from 'svelte/transition';

    // Starting progress
    let progress = 0; 

    // Duration of the progress in milliseconds
    const duration = 3600; 

    onMount(() => {
    const interval = setInterval(() => {
        progress += 100 / (duration / 100); // Increment progress
        if (progress >= 100) {
        clearInterval(interval);
        progress = 100;
        }
    }, 100); // Update every (value)ms
    });
</script>

<div out:fade={{ duration: 2400 }}>
    <div class="preloader flex justify-center items-center h-screen">
        <div class="absolute"> 
            <h1 class="text-white">Your Adventure is About to Begin...</h1>
            <progress class="nes-progress is-primary" value="{progress}" max="100"></progress>
        </div>
    </div> 
</div>