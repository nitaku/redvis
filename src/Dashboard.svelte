<script lang="ts">
	import DashboardCell from './DashboardCell.svelte'
	export let cells: {id: number; code: string}[] = []
</script>

<main>
	{#each cells as cell(cell.id)}
		<DashboardCell cell={cell}/>
	{/each}
</main>

<style>
	main {
		background: whitesmoke;
        flex-grow: 1;
		display: flex;
		flex-direction: column;
		overflow: auto;
	}
</style>