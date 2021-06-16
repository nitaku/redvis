<script lang="ts">
    import {dndzone} from "svelte-dnd-action"
    import {flip} from "svelte/animate"

    import EditorCell from './EditorCell.svelte'

    export let cells: {id: number; code: string}[] = []
    export let visible

    const flipDurationMs = 200
    function handleSort(e: CustomEvent<DndEvent>) {
        cells = e.detail.items as {id: number; code: string}[];
    }
</script>

<main use:dndzone="{{items: cells, flipDurationMs}}" on:consider="{handleSort}" on:finalize="{handleSort}" class:visible>
    {#each cells as cell(cell.id)}
        <div animate:flip="{{duration:flipDurationMs}}">
            <EditorCell bind:cell={cell} on:run/>
        </div>
    {/each}
</main>

<style>
    main.visible {
        width: 350px;
        min-width: 350px;
        display: flex;
        flex-direction: column;
        overflow-y: scroll;
        overflow-x: hidden;
    }
    main :last-child {
        margin-bottom: 18px;
    }
    main:not(.visible) {
        display: none;
    }
</style>