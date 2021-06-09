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
		background: white;
        min-width: 350px;
        resize: none;
        margin: 0;
        font-family: monospace;
	}
</style>