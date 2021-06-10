<script lang="ts">
    import {dndzone} from "svelte-dnd-action"
    import {flip} from "svelte/animate"

    import Cell from './Cell.svelte'

    export let cells: {id: number; code: string}[] = []

    const flipDurationMs = 200
    function handleSort(e: CustomEvent<DndEvent>) {
        cells = e.detail.items as {id: number; code: string}[];
    }
</script>

<main use:dndzone="{{items: cells, flipDurationMs}}" on:consider="{handleSort}" on:finalize="{handleSort}">
    {#each cells as cell(cell.id)}
        <div animate:flip="{{duration:flipDurationMs}}">
            <Cell bind:code={cell.code} on:run/>
        </div>
    {/each}
</main>

<style>
    main {
        width: 350px;
        display: flex;
        flex-direction: column;
        overflow-y: scroll;
        overflow-x: hidden;
    }
    main :last-child {
        margin-bottom: 18px;
    }
</style>