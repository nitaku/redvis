<script lang="ts">
    import Cell from './Cell.svelte'

    export let cells = []
</script>

<main>
    {#each cells as cell}
        <Cell bind:code={cell.code} on:run/>
    {/each}
</main>

<style>
    main {
        display: flex;
        flex-direction: column;
        overflow-y: scroll;
    }
</style>