<script lang="ts">
	import type monaco from 'monaco-editor';
	import { onMount } from 'svelte';

	export let value: string;
	export let readOnly: boolean = false;

	let divElement: HTMLElement;
	let editor: monaco.editor.IStandaloneCodeEditor;
	let monacoLoading: boolean = true;

	$: readOnly && editor?.setValue(value);

	onMount(async () => {
		const monaco = await import('monaco-editor');
		editor = monaco.editor.create(divElement, {
			value,
			language: 'javascript',
			readOnly,
			automaticLayout: true,
			contextmenu: false,
			quickSuggestions: false,
			scrollBeyondLastLine: false,
			minimap: {
				enabled: false
			}
		});
		editor.onDidChangeModelContent(() => {
			value = editor.getValue();
		});

		monacoLoading = false;
		return () => editor.dispose();
	});
</script>

<div bind:this={divElement} class:monacoLoading>
	<textarea bind:value class:monacoLoading />
</div>

<style>
	div,
	textarea {
		width: 100%;
		height: 100%;
	}
	div {
		border: 1px solid black;
		overflow: hidden;
	}
	textarea {
		border: none;
		display: none;
	}
	textarea.monacoLoading {
		display: block;
	}
</style>
