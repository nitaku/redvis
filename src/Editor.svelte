<script lang="ts">
    export let code = `a = 1;
viewof b = html\`<input type=range>\`; 
c = a + b;

height = 200
viewof color = html\`<input type=color value=#c6e89a>\`
import {chart} with {height, color} from '@d3/bar-chart'
chart`
</script>

<textarea on:input bind:value={code}></textarea>

<style>
	textarea {
		background: white;
        width: 400px;
        resize: none;
        margin: 0;
        font-family: monospace;
	}
</style>