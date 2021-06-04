<script lang="ts">
</script>

<main>
	<slot></slot>
</main>

<style>
	main {
		background: whitesmoke;
        flex-grow: 1;
	}
	main :global(.observablehq) {
		margin: 4px;
		padding: 4px;
		background: white;
		border-bottom: 1px solid #DDD;
		border-radius: 4px;
	}
</style>