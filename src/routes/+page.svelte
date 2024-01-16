<script lang="ts">
    import { onMount } from 'svelte';
    import Preloader from '../components/Preloader.svelte';
    import Intro from '../components/Intro.svelte';
    import background from '$lib/background.png';

    let isLoading = true;

    onMount(() => {
      setTimeout(() => { isLoading = false; }, 4800);      
    });

</script>

<div class="relative">
  <div class="fixed top-0 left-0 w-full h-full bg-fixed bg-cover brightness-50" style="background-image:url({background})"></div>
  {#if isLoading}
    <div class="absolute z-20 top-0 left-0 w-full h-full">
      <Preloader />
    </div>
    
  {:else}
    <div class="absolute z-10 top-0 left-0 w-full h-full">
      <Intro />
    </div>
  {/if}
</div>