<script lang="ts">
	import { onMount } from 'svelte'

	import { Interpreter } from '@alex.garcia/unofficial-observablehq-compiler'
	import { Runtime, Inspector } from '@observablehq/runtime'

	import View from './View.svelte'
	import Editor from './Editor.svelte'

	let content
	let program

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

		interpreter.module(program)
	}
</script>

<main>
	<Editor bind:code={program} on:run={run}/>
	<View>
		<div bind:this={content}></div>
	</View>
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