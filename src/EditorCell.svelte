<script lang="ts">
    import { onMount } from 'svelte'
    import { createEventDispatcher } from 'svelte'

    import {EditorState} from "@codemirror/state"
    import {EditorView, keymap} from "@codemirror/view"
    import {defaultKeymap} from "@codemirror/commands"
    import {defaultHighlightStyle} from "@codemirror/highlight"
    import {bracketMatching} from "@codemirror/matchbrackets"
    import {javascript} from "@codemirror/lang-javascript"
    import {history, historyKeymap} from "@codemirror/history"
    
    let editorWrapper
    let view

    const dispatch = createEventDispatcher()

    export let cell

    onMount(async () => {
        let startState = EditorState.create({
            doc: cell.code,
            extensions: [
                keymap.of([
                    ...defaultKeymap,
                    ...historyKeymap
                ]),
                defaultHighlightStyle.fallback,
                javascript(),
                bracketMatching(),
                EditorView.lineWrapping,
                history(),
                EditorView.updateListener.of(update => {
                    cell.code = update.state.doc.toString()
                    
                    // handle focus and blur events
                    if(update.focusChanged) {
                        cell.focus = update.view.hasFocus
                        if(!cell.focus) {
                            // run cell on blur
                            run()
                        }
                    }
                })
            ]
        })

        view = new EditorView({
            state: startState,
            parent: editorWrapper
        })
    })

    function handleKeyup(event) {
        if (event.ctrlKey && event.code == 'Enter') {
            run()
        }
    }
    function run() {
        dispatch('run', cell)
    }
</script>

<main class="cell" on:keyup={handleKeyup}>
    <div class="handle material-icons">code</div>
    <div class="editorWrapper" bind:this={editorWrapper}></div>
</main>

<style>
    main {
        margin: 8px;
        margin-bottom: 0;
        border-radius: 4px;
        display: flex;
        flex-direction: row;
        background: #ebf0f3;
        box-shadow: 0px 1px 2px grey;
	}
    .handle {
        color: #c0dee9;
        margin-left: 2px;
        margin-right: -2px;
    }
    .editorWrapper {
        flex-grow: 1;
        margin-left: 2px;
        cursor: text;
    }
    main:hover .handle {
        color: #96afb8;
    }
    :global(#dnd-action-dragged-el) .handle {
		color: black;
	}
    main :global(.cm-editor.cm-focused) {
        outline: 1px solid black;
    }
</style>