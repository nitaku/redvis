<script lang="ts">
	import { max } from 'd3-array'
	import { onMount } from 'svelte'

	import { Interpreter } from '@alex.garcia/unofficial-observablehq-compiler'
	import { Runtime, Inspector } from '@observablehq/runtime'

	import Bar from './Bar.svelte'
	import Dashboard from './Dashboard.svelte'
	import Editor from './Editor.svelte'

	let cells = [
        {id: 1, code: `a = width`, variables: [], focus: false},
        {id: 2, code: `b = 2`, variables: [], focus: false},
        {id: 3, code: `c = a+b`, variables: [], focus: false}
    ]
	let runtime
	let main

	onMount(async () => {
		runtime = new Runtime()
		main = runtime.module()
	})

	function handleRun(e) {
		runCell(e.detail)
	}
	async function runCell(cell) {
		// delete old variables, if any
		cell.variables.forEach(v => v.delete())

		cell.dashboardCell.innerHTML = ''
		const observer = Inspector.into(cell.dashboardCell)

		const interpreter = new Interpreter({
			observeViewofValues: false,
			module: main,
			observer
		})

		// TODO: width and height injection

		cell.variables = await interpreter.cell(cell.code)
	}
	function newCell() {
		cells = cells.concat([{
			id: max(cells, d => d.id)+1,
			code: '',
			variables: [],
			focus: false
		}])
	}
	function handleAdd() {
		newCell()
	}
</script>

<main>
	<Bar>
		<button on:click={handleAdd}>+</button>
	</Bar>
	<div style="display: flex; flex-grow: 1; overflow: hidden;">
		<Editor bind:cells={cells} on:run={handleRun}/>
		<Dashboard bind:cells={cells}/>
	</div>
</main>

<style>
	:global(html), :global(body) {
		padding: 0;
		margin: 0;
		overflow: hidden;
	}
	main {
		display: flex;
		flex-direction: column;
		width: 100%;
		height: 100%;
	}
</style>