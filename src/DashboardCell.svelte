<script lang="ts">
	import { onMount } from 'svelte'

    export let cell
	let self

	onMount(async () => {
		cell.dashboardCell = self
	})
</script>

<main bind:this={self}>
</main>

<style>
    main {
		margin: 4px;
		padding: 4px;
		background: white;
		border-bottom: 1px solid #DDD;
		border-radius: 4px;
		flex-grow: 1;
		flex-basis: 0;
	}
</style>