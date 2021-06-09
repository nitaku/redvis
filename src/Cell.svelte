<script lang="ts">
    import { createEventDispatcher } from 'svelte'

    const dispatch = createEventDispatcher()

    export let code = ``

    function handleKeyup(event) {
        if (event.ctrlKey && event.code == 'Enter') {
            dispatch('run')
        }
    }
</script>

<textarea on:keyup={handleKeyup} bind:value={code}></textarea>

<style>
	textarea {
		background: whitesmoke;
        min-width: 350px;
        resize: none;
        margin: 4px;
        margin-bottom: 0;
        border: 0;
        border-radius: 0;
        font-family: monospace;
	}
</style>