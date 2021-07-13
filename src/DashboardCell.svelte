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
		let lastClientRect
		new ResizeSensor(self, () => {
			const clientRect = self.getBoundingClientRect()
			
			// skip resizing if the element has not changed size
			if (lastClientRect !== undefined && clientRect.width === lastClientRect.width && clientRect.height === lastClientRect.height) return

			lastClientRect = clientRect

			// re-run the cell on resize
			run()
		})
	})

	function run() {
        dispatch('run', cell)
    }
</script>

<main bind:this={self} style="grid-area: _{cell.id};" class:focus={cell.focus}>
	<div class="content" bind:this={content}></div>
</main>

<style>
    main {
		padding: 6px;
		background: white;
		box-shadow: 0 1px 2px 1px #DDD;
		border-radius: 6px;
		flex-grow: 1;
		flex-basis: 0;
		overflow: auto;
	}
	main.focus {
		box-shadow: 0 0 0 1px black;
	}
	/* code preview */
	main {
		font-family: monospace;
		white-space: pre;
	}
	main :global(.observablehq--number) {
		color: #b75501;
	}
</style>