<script lang="ts">
</script>

<main>
	RED<br/>
	VIS
	<slot>
	</slot>
</main>

<style>
	main {
		background: #c6d2d7;
	}
	main > :global(*) {
		display: block;
	}
</style>