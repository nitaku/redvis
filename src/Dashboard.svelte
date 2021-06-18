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
		background: #f9fbfcff;
		overflow: auto;
		flex-grow: 1;
		display: grid;
		padding: 3px;
		grid-template-columns: 1fr 1fr;
		grid-template-rows: 2fr 1fr;
		gap: 0px 0px;
		grid-auto-flow: row;
		grid-template-areas:
			"_1 _1"
			"_2 _3";
		}
</style>