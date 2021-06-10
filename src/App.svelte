<script lang="ts">
	import { onMount } from 'svelte'

	import { Interpreter } from '@alex.garcia/unofficial-observablehq-compiler'
	import { Runtime, Inspector } from '@observablehq/runtime'

	import Bar from './Bar.svelte'
	import View from './View.svelte'
	import Editor from './Editor.svelte'

	let editorCells = [
        {id: 1, code: `a = 1`},
        {id: 2, code: `b = 2`},
        {id: 3, code: `c = a+b`}
    ]
	let content

	onMount(async () => {
		run()
	})

	function run() {
		content.innerHTML = ''
		
		const runtime = new Runtime()
		const main = runtime.module()
		const observer = Inspector.into(content)

		const interpreter = new Interpreter({
			observeViewofValues: false,
			module: main,
			observer
		})

		interpreter.module(editorCells.map(d => d.code).join('\n'))
	}
</script>

<main>
	<Bar/>
	<div style="display: flex; flex-grow: 1; overflow: hidden;">
		<Editor bind:cells={editorCells} on:run={run}/>
		<View bind:content={content}/>
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