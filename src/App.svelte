<script lang="ts">
	import View from './View.svelte'
	import Editor from './Editor.svelte'
</script>

<main>
	<View/><Editor/>
</main>

<style>
	:global(html), :global(body) {
		padding: 0;
		margin: 0;
		overflow: hidden;
	}
	main {
		display: flex;
		width: 100%;
		height: 100%;
	}
</style>