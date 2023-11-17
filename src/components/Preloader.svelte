<script>
   import { fade } from 'svelte/transition';
</script>
  
<div in:fade={{ duration: 2400 }} out:fade={{ duration: 2400 }} class="preloader flex justify-center items-center h-screen">
    Loading...
</div> 