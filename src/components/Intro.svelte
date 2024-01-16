<script lang="ts">
    import { onMount } from 'svelte';
    import { writable } from 'svelte/store';
    import { fly } from 'svelte/transition';
    import { goto } from '$app/navigation';

    const messages = [
        "",
        "Welcome to my website!",
        "My name is Jordan, it's lovely to meet you.",
        "I'm here to help you navigate my village...",
        "Here you can explore and learn more about me...",
        "So, where would you like to explore first?"
    ];

    const message = writable(messages[0]);
    let lastMessageDisplayed = false;

    function lastMessage() {
    lastMessageDisplayed = true;
    }

    const changeMessage = () => {
    let i = 0;
    let j = 0;
    let currentMessage = '';
    let interval: any;

    const typeMessage = () => {
        interval = setInterval(() => {
            if (j < messages[i].length) {
                currentMessage += messages[i][j];
                $message = currentMessage;
                j++;
            } else {
                clearInterval(interval);
                i++;
                j = 0;
                currentMessage = '';
                if (i < messages.length) {
                    setTimeout(typeMessage, 3000);
                } else {
                    lastMessage();
                }
            }
        }, 100);
    }

    typeMessage();
    }

    function navigate(route: string) {
    goto(`/${route}`);
    }

    onMount(() => { 
      setTimeout(changeMessage, 2000);   
    });

</script>



<div in:fly={{ delay: 3000, duration: 3000, y: 100 }}>
    <div class="w-2/3 m-auto">
        <div class="h-screen grid grid-cols-1 content-center">
            <div class="grid justify-items-end">
                {#if lastMessageDisplayed}
                    <div in:fly={{ delay: 500, duration: 2000, y: -100 }} class="nes-balloon from-right">
                        <button on:click={() => navigate('farm')} title="Go to the farm" type="button" class="nes-btn is-success">The Farm</button>
                        <button on:click={() => navigate('blacksmith')} type="button" class="nes-btn is-success">The Blacksmith</button>
                        <button on:click={() => navigate('notice-board')} type="button" class="nes-btn is-success">Notice Board</button>
                    </div>
                {/if}
            </div>
            <section class="message -left">
                <div class="nes-balloon from-left">
                    <p>{$message}</p>
                </div>
            </section>
            <i class="nes-octocat animate"></i>
        </div>
    </div>
</div>