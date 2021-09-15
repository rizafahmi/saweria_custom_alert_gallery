<script>
 import client from '../client.js';
 import { onMount } from 'svelte';

 let alerts = [];

 onMount(async function() {
     const { data, error } = await client
         .view("allGallery")
         .readRows({ offset: 0, limit: 10, order: "desc" })
         .toPromise();
     alerts = await data.nodes;

 });

</script>
<h1>Alert Gallery</h1>

<ul>
    {#each alerts as alert}
        <li>{alert.title}</li>
    {/each}
</ul>
