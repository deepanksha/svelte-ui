<script>
	import { Card, Button, Spinner } from 'flowbite-svelte';
	import { onMount } from 'svelte';
	import EditModel from '../../components/EditModel.svelte';

	let data = null; // Set initial state to null
	let error = null;
	let showModal = false;
	let selectedCard = null;

	onMount(async () => {
		try {
			const response = await fetch('https://jsonplaceholder.typicode.com/posts');
			if (!response.ok) {
				throw new Error('Network response was not ok');
			}
			data = await response.json();
			console.log(data);
		} catch (err) {
			error = err.message;
		}
	});

	function openModal(card) {
		selectedCard = { ...card }; // Create a copy of the card to edit
		showModal = true;
	}

	function closeModal() {
		showModal = false;
	}

	function saveChanges(event) {
		const updatedCard = event.detail;
		const index = data.findIndex((item) => item.id === updatedCard.id);
		data = [...data.slice(0, index), updatedCard, ...data.slice(index + 1)];
		closeModal();
	}

	function deleteCard(id) {
		data = data.filter((item) => item.id !== id);
	}
</script>

<main class=" h-screen w-screen overflow-x-hidden bg-slate-200">
	{#if data === null}
		<div class="flex h-screen w-screen items-center justify-center bg-fuchsia-400 text-center">
			<Spinner size={32} />
		</div>
	{:else if data.length > 0}
		<div class="my-16 flex flex-wrap items-center justify-center gap-6">
			{#each data as item}
				<Card padding="md" class="min-h-80">
					<div class="flex flex-col items-center pb-4">
						<h5 class="mb-1 text-xl font-medium text-gray-900 dark:text-white">{item.title}</h5>
						<span class="text-sm text-gray-500 dark:text-gray-400">{item.body}</span>
						<div class="mt-4 flex space-x-3 lg:mt-6 rtl:space-x-reverse">
							<Button on:click={() => openModal(item)}>Edit</Button>
							<Button color="light" class="dark:text-white" on:click={() => deleteCard(item.id)}
								>Delete</Button
							>
						</div>
					</div>
				</Card>
			{/each}
		</div>
	{:else}
		<div class="text-center">No data available.</div>
	{/if}

	{#if showModal}
		<EditModel {selectedCard} on:close={closeModal} on:save={saveChanges} />
	{/if}
</main>

<style>
	/* Add your styles here */
</style>
