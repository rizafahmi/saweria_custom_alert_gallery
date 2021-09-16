<script>
 import { onMount } from 'svelte';
 import client from '../client.js';

 let alerts = [];

 onMount(async function() {
     const { data, error } = await client
         .view("allGallery")
         .readRows({ offset: 0, limit: 10, order: "desc" })
         .toPromise();
     alerts = await data.nodes;

 });

</script>
<header>
    <h1>Alert Gallery</h1>
</header>
<main>
    <article>
            {#each alerts as alert}
                <h3>{alert.title}</h3>
                <div class="alert">
                    <div class="thumbnail"><img alt={alert.title + " preview"} src={alert.thumbnail} /></div>
                    <div class="codes">
                        <div class="code-html">
                            <h4>HTML</h4>
                            <button>Copy</button>
                            <textarea name="css" id="" rows="6" cols="48" tabindex="" readonly>{alert.html}</textarea>
                        </div>
                        <div class="code-css">
                            <h4>CSS</h4>
                            <button>Copy</button>
                            <textarea name="css" id="" rows="6" cols="48" tabindex="" readonly >{alert.css}</textarea>
                        </div>
                    </div>
                </div>
                <hr />

            {/each}
    </article>
</main>

<style type="text/css" media="screen">
 /* Sumber: https://moderncss.dev/css-button-styling-guide/ */
    img {
    border: 2px solid red;
    border-radius: 8px;
    width: 200px;
    height: auto;
    }
    .alert {
    display: flex;
    align-items: center;
    }
    .thumbnail {
    flex: 1;
    padding-right: 1rem;
    }
    .codes {
    flex: 3;
    }

    textarea {
    resize: vertical;
    font-size: 16px;
    font-size: max(16px, 1em);
    font-family: inherit;
    padding: 0.25em 0.5em;
    background-color: #fff;
    border: 2px solid #8b8a8b;
    border-radius: 4px;
    }
    textarea:focus {
    border-color: hsl(245, 100%, 42%);
    box-shadow: 0 0 0 3px hsla(245, 100%, calc(42% + 40%), 0.8);
    transition: 180ms box-shadow ease-in-out;
    outline: 3px solid transparent;
    }
    button {
    box-sizing: border-box;
    border: none;
    background-color: transparent;
    font-family: inherit;
    font-size: 0.75rem;
    padding: 0;
    cursor: pointer;
    display: inline-flex;
    align-items: center;
    justify-content: center;
    background-color: #3e68ff;
    color: #fff;
    border-radius: 8px;
    box-shadow: 0 3px 5px rgba(0, 0, 0, 0.18);
    /* padding: 0.25em 0.75em; */
 min-width: 8ch;
     min-height: 32px;
     text-align: center;
     line-height: 1;

     /* @media screen and (-ms-high-contrast: active) {
        border: 2px solid currentcolor;
        } */
 }
</style>
