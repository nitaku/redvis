<script lang="ts">
    import { onMount } from 'svelte'
    import { createEventDispatcher } from 'svelte'

    import {EditorState} from "@codemirror/state"
    import {EditorView, keymap} from "@codemirror/view"
    import {defaultKeymap} from "@codemirror/commands"
    import {defaultHighlightStyle} from "@codemirror/highlight"
    import {bracketMatching} from "@codemirror/matchbrackets"
    import {javascript} from "@codemirror/lang-javascript"
    
    let main
    let view

    const dispatch = createEventDispatcher()

    export let code = ``

    function handleKeyup(event) {
        if (event.ctrlKey && event.code == 'Enter') {
            dispatch('run')
        }
    }

    onMount(async () => {
        let startState = EditorState.create({
            doc: code,
            extensions: [
                keymap.of(defaultKeymap),
                defaultHighlightStyle.fallback,
                javascript(),
                bracketMatching(),
                EditorView.lineWrapping,
                EditorView.updateListener.of(update => {
                    code = update.state.doc.toString()
                })
            ]
        })

        view = new EditorView({
            state: startState,
            parent: main
        })
    })
</script>

<main bind:this={main} on:keyup={handleKeyup}>
    <span class="handle material-icons">drag_indicator</span>
</main>

<style>
    main {
        margin: 4px;
        margin-bottom: 0;
	}
    .handle {
        float: left;
        color: #DDD;
        visibility: hidden;
    }
    main :global(.cm-editor) {
        background: whitesmoke;
        border-left: 2px solid whitesmoke;
    }
    main:hover .handle {
        visibility: visible;
    }
    main:hover :global(.cm-editor) {
        border-left: 2px solid #DDD;
    }
</style>