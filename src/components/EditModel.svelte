<script>
	import { createEventDispatcher } from 'svelte';

	export let selectedCard;

	const dispatch = createEventDispatcher();

	let title = selectedCard.title;
	let body = selectedCard.body;

	function save() {
		dispatch('save', { ...selectedCard, title, body });
	}

	function close() {
		dispatch('close');
	}
</script>

<div class="fixed inset-0 flex items-center justify-center bg-black bg-opacity-50">
	<div class="w-96 rounded bg-white p-4 shadow-lg">
		<h2 class="mb-4 text-lg font-bold">Edit Post</h2>
		<label class="mb-2 block">
			Title:
			<input type="text" bind:value={title} class="mt-1 block w-full rounded border p-2" />
		</label>
		<label class="mb-4 block">
			Body:
			<textarea bind:value={body} class="mt-1 block w-full rounded border p-2"></textarea>
		</label>
		<div class="flex justify-end space-x-2">
			<button on:click={close} class="rounded bg-gray-300 px-4 py-2">Cancel</button>
			<button on:click={save} class="rounded bg-blue-600 px-4 py-2 text-white">Save</button>
		</div>
	</div>
</div>
