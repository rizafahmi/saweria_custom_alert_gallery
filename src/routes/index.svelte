<script>
 import { onMount } from 'svelte';
 import { selectTextOnFocus } from '$lib/inputDirectives.js';
 import Header from '../components/header.svelte';

 import client from '../client.js';

 let alerts = [];
 let textareaCss, textareaHtml;

 function copyCss() {
     console.log("Copying css..");
     textareaCss.select();
     document.execCommand('copy');
     textareaCss.selectionStart = textareaCss.selectionEnd = 1;
 }
 function copyHtml() {
     console.log("Copying html..");
     textareaHtml.select();
     document.execCommand('copy');
     textareaHtml.selectionStart = textareaHtml.selectionEnd = 1;

 }

 onMount(async function() {
     const { data, error } = await client
         .view("allGallery")
         .readRows({ offset: 0, limit: 10, order: "desc" })
         .toPromise();
     alerts = await data.nodes;

 });

</script>
<Header />
<main>
    <article>
            {#each alerts as alert}
                <h4>{alert.title}</h4>
                <div class="alert">
                    <div class="thumbnail"><img alt={alert.title + " preview"} src={alert.thumbnail} /></div>
                    <div class="codes">
                        <div class="code-html">
                            <h4>HTML</h4>
                            <button on:click={copyHtml}>Copy</button>
                            <textarea bind:this={textareaHtml} name="html" id="" rows="6" cols="48" tabindex="" readonly use:selectTextOnFocus>{alert.html}</textarea>
                        </div>
                        <div class="code-css">
                            <h4>CSS</h4>
                            <button on:click={copyCss}>Copy</button>
                            <textarea bind:this={textareaCss} name="css" id="" rows="6" cols="48" tabindex="" readonly use:selectTextOnFocus>{alert.css}</textarea>
                        </div>
                    </div>
                </div>
                <hr />

            {/each}
    </article>
</main>

<style type="text/css" media="screen">
 /* Sumber: https://moderncss.dev/css-button-styling-guide/ */
 .thumbnail > img {
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
