<script lang="ts">
	import { ResizeSensor } from 'css-element-queries'

	import { onMount } from 'svelte'
	import { createEventDispatcher } from 'svelte'

    export let cell
	let self
	let content

	const dispatch = createEventDispatcher()

	onMount(async () => {
		cell.dashboardCell = self
		cell.content = content

		// wait an animation frame to let ElementQueries do its thing
		await new Promise(r => requestAnimationFrame(r))

		// listen to element resizing
		new ResizeSensor(self, () => {
			// re-run the cell on resize
			run()
		})
	})

	function run() {
        dispatch('run', cell)
    }
</script>

<main bind:this={self} class:focus={cell.focus}>
	<div class="content" bind:this={content}></div>
</main>

<style>
    main {
		margin: 6px;
		padding: 6px;
		background: white;
		box-shadow: 0 1px 2px 1px #DDD;
		border-radius: 6px;
		flex-grow: 1;
		flex-basis: 0;
	}
	main:not(:last-child) {
		margin-bottom: 0;
	}
	main.focus {
		box-shadow: 0 0 0 1px black;
	}
</style>