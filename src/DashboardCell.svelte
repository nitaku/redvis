<script lang="ts">
	import { onMount } from 'svelte'

    export let cell
	let self

	onMount(async () => {
		cell.dashboardCell = self
	})
</script>

<main bind:this={self} style="grid-area: _{cell.id};" class:focus={cell.focus}>
</main>

<style>
    main {
		margin: 4px;
		padding: 6px;
		background: white;
		box-shadow: 0 1px 2px 1px #DDD;
		border-radius: 6px;
		flex-grow: 1;
		flex-basis: 0;
	}
	main.focus {
		box-shadow: 0 0 0 1px black;
	}
</style>