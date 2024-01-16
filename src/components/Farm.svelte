<script lang='ts'>
    import { onMount } from 'svelte';
    import Repo from '../components/Repo.svelte';

    let repos: { 
        html_url: string, 
        name: string, 
        description: string,
        language: string 
    }[] = [];


    onMount(async () => {
    const response = await fetch('https://api.github.com/users/JordanRobo/repos');
    let allRepos = await response.json();
    repos = allRepos.filter((repo: any) => repo.name && repo.description && repo.language);
    console.log(repos);
    });
</script>

<h1>My GitHub Repositories</h1>

<ul>
    {#each repos as repo}
    <Repo>
        <h2 slot="title">{repo.name}</h2>
        <p slot="description">{repo.description}</p>
        <p slot="language">{repo.language}</p>
        <a class="text-white" href={repo.html_url} slot="button" target="_blank">View Repo</a>
    </Repo>
    {/each}
</ul>
