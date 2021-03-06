<script lang="ts">
	import { ElementQueries } from 'css-element-queries'

	import { max } from 'd3-array'
	import { onMount } from 'svelte'

	import { Interpreter } from '@alex.garcia/unofficial-observablehq-compiler'
	import { Runtime, Inspector } from '@observablehq/runtime'

	import Bar from './Bar.svelte'
	import Dashboard from './Dashboard.svelte'
	import Editor from './Editor.svelte'

	let cells = [
        {id: 1, code: `a = 1`, variables: [], focus: false},
        {id: 2, code: `b = 2`, variables: [], focus: false},
        {id: 3, code: `c = a+b`, variables: [], focus: false}
    ]
	let runtime
	let main
	let editorVisible = true
	
	onMount(async () => {
		// enable resize event listening
		ElementQueries.listen()

		runtime = new Runtime()
		main = runtime.module()

		// run the entire program at startup
		cells.forEach(cell => runCell(cell))
	})

	function handleRun(e) {
		runCell(e.detail)
	}
	async function runCell(cell) {
		// delete old variables, if any
		cell.variables.forEach(v => v.delete())

		cell.content.innerHTML = ''
		const observer = Inspector.into(cell.content)

		const interpreter = new Interpreter({
			observeViewofValues: false,
			module: main,
			observer
		})

		let code = cell.code
			.replaceAll(/redvis/g, '('+JSON.stringify({
				cell: {
					id: cell.id,
					size: getInnerSize(cell.dashboardCell)
				}
			})+')')

		cell.variables = await interpreter.cell(code)
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
	function handleHide() {
		editorVisible = !editorVisible
	}

	function getInnerSize( element ){
		let computed = getComputedStyle(element)
		let paddingWidth = parseFloat(computed.paddingLeft) + parseFloat(computed.paddingRight)
		let paddingHeight = parseFloat(computed.paddingTop) + parseFloat(computed.paddingBottom)
		
		return {
			width: element.clientWidth-paddingWidth,
			height: element.clientHeight-paddingHeight
		}
	}
</script>

<main>
	<Bar>
		<button on:click={handleAdd} class="material-icons">add</button>
		<button on:click={handleHide} class="material-icons">code</button>
	</Bar>
	<div style="display: flex; flex-grow: 1; overflow: hidden;">
		<Editor bind:cells={cells} on:run={handleRun} visible={editorVisible}/>
		<Dashboard bind:cells={cells} on:run={handleRun}/>
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
		flex-direction: row;
		width: 100%;
		height: 100%;
	}
</style>