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
		background: whitesmoke;
	}
	main > :global(*) {
		display: block;
	}
</style>