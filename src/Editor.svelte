<script lang="ts">
    import { createEventDispatcher } from 'svelte'

    const dispatch = createEventDispatcher()

    export let code = `a = 1;
viewof b = html\`<input type=range>\`; 
c = a + b;

height = 200
viewof color = html\`<input type=color value=#c6e89a>\`
import {chart} with {height, color} from '@d3/bar-chart'
chart`

    function handleKeyup(event) {
        if (event.ctrlKey && event.code == 'Enter') {
            dispatch('run')
        }
    }
</script>

<textarea on:keyup={handleKeyup} bind:value={code}></textarea>

<style>
	textarea {
		background: white;
        min-width: 350px;
        resize: none;
        margin: 0;
        font-family: monospace;
	}
</style>