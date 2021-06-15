<script lang="ts">
	import { onMount } from 'svelte'

    export let cell
	let self

	onMount(async () => {
		cell.dashboardCell = self
	})
</script>

<main bind:this={self} class:focus={cell.focus}>
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