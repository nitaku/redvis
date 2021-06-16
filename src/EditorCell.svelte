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
    <span class="handle material-icons">drag_indicator</span>
    <div class="editorWrapper" bind:this={editorWrapper}></div>
</main>

<style>
    main {
        margin: 4px;
        margin-bottom: 0;
        display: flex;
        flex-direction: row;
        background: whitesmoke;
	}
    .handle {
        color: #CCC;
        visibility: hidden;
    }
    .editorWrapper {
        flex-grow: 1;
        border-left: 2px solid whitesmoke;
        cursor: text;
    }
    main:hover .handle {
        visibility: visible;
    }
    main:hover .editorWrapper {
        border-left: 2px solid #CCC;
    }
    :global(#dnd-action-dragged-el) .handle {
        visibility: visible;
		color: black;
	}
    :global(#dnd-action-dragged-el) .editorWrapper {
        border-left: 2px solid black;
	}
    main :global(.cm-editor.cm-focused) {
        outline: 1px solid black;
    }
</style>