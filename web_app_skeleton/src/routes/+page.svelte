<!-- YOU CAN DELETE EVERYTHING IN THIS PAGE -->
<script lang="ts">
	import { onMount } from 'svelte';
	// german | english
	let selected_langauge: String;
	// yes | no
	let selected_letter_count: String;
	// text
	let selected_text: String;
	let german_dictionary: Set<String>;
	let english_dictionary: Set<String>;

	onMount(async () => {
		try {
			const response = await fetch('/path/to/file.txt');
			const fileContents = await response.text();
			// Perform operations with the file contents here
			console.log(fileContents);
		} catch (error) {
			console.error(error);
		}
	});

	const handle_keydown = (event: { key: string }) => {
		if (event.key === 'Enter') {
			// Handle the "Enter" key press event here
			console.log(selected_text);
			selected_text = '';
		}
	};

	// Todo make app work
	const check_spelling = (input: String): [String] => {
		return ['Hello'];
	};
</script>

<div on:keydown={handle_keydown} class="container h-full mx-auto flex justify-center items-center">
	<div class="space-y-10 text-center flex flex-col items-center">
		<h2 class="h1">Check that spelling !</h2>
		<!-- Animated Logo -->
		<section class="img-bg" />
		<form class="form" />
		<div class="card p-4 w-full text-token space-y-4">
			<label class="label">
				<span>Langauge</span>
				<select bind:value={selected_langauge} class="select">
					<option value="german">German</option>
					<option value="englih">English 󠁧󠁢󠁥󠁮󠁧󠁿</option>
				</select>
			</label>
			<label class="label">
				<span>Letter count</span>
				<select bind:value={selected_letter_count} class="select">
					<option value="yes">Yes please</option>
					<option value="no">No thanks󠁧</option>
				</select>
			</label>
			<label class="label">
				<span>Text</span>
				<textarea
					bind:value={selected_text}
					class="textarea"
					rows="4"
					placeholder="Text to check out 👀"
				/>
			</label>
		</div>

		<!-- / -->
		<div class="flex justify-center space-x-2">
			<button on:click={console.log(selected_text)} type="button" class="btn variant-filled">
				<span>Check spelling</span>
			</button>
		</div>
		<div class="space-y-2">
			<!-- <p>Try editing the following:</p> -->
		</div>
	</div>
</div>

<style lang="postcss">
	figure {
		@apply flex relative flex-col;
	}
	figure svg,
	.img-bg {
		@apply w-64 h-64 md:w-80 md:h-80;
	}
	.img-bg {
		@apply absolute z-[-1] rounded-full blur-[50px] transition-all;
		animation: pulse 5s cubic-bezier(0, 0, 0, 0.5) infinite, glow 5s linear infinite;
	}
	@keyframes glow {
		0% {
			@apply bg-primary-400/50;
		}
		33% {
			@apply bg-secondary-400/50;
		}
		66% {
			@apply bg-tertiary-400/50;
		}
		100% {
			@apply bg-primary-400/50;
		}
	}
	@keyframes pulse {
		50% {
			transform: scale(1.5);
		}
	}
</style>
