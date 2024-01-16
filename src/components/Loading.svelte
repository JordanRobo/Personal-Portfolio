<script lang="ts">
    import { onMount } from 'svelte';
    import { fade } from 'svelte/transition';
    import tips from '$lib/tips';
 
     // Starting progress
    let progress = 0; 
 
     // Duration of the progress in milliseconds
    const duration = 4200; 

    let tip = randomTip();

    function randomTip() {
        const randomIndex = Math.floor(Math.random() * tips.length);
        return tips[randomIndex];
    }
 
     onMount(() => {
     const interval = setInterval(() => {
         progress += 100 / (duration / 100); // Increment progress
         if (progress >= 100) {
         clearInterval(interval);
         progress = 100;
         }
     }, 200); // Update every (value)ms
     });
 </script>
 
 <div out:fade={{ duration: 2400 }}>
     <div class="flex justify-center items-center h-screen">
         <div class="absolute"> 
            <h1 class="text-white">{tip}</h1>
            <progress class="nes-progress is-primary" value="{progress}" max="100"></progress>
         </div>
     </div> 
 </div>