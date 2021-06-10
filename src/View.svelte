<script lang="ts">
	export let content
</script>

<main bind:this={content}>
</main>

<style>
	main {
		background: whitesmoke;
        flex-grow: 1;
		display: flex;
		flex-direction: column;
		overflow: auto;
	}
	main :global(.observablehq) {
		margin: 4px;
		padding: 4px;
		background: white;
		border-bottom: 1px solid #DDD;
		border-radius: 4px;
		flex-grow: 1;
		flex-basis: 0;
	}
</style>