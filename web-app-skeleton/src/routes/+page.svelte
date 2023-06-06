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
	let wrong_words_finish = 'All correct ✅';

	onMount(async () => {
		try {
			let response = await fetch('english_dict.txt');
			let fileContents = await response.text();
			// Perform operations with the file contents here
			english_dictionary = new Set(fileContents.split('\n'));

			response = await fetch('ger_dict.txt');
			fileContents = await response.text();
			german_dictionary = new Set(fileContents.split('\n'));
			console.log('loaded Set');
		} catch (error) {
			console.error(error);
		}
	});

	const handle_keydown = (event: { key: string }) => {
		if (event.key === 'Enter') {
			// Handle the "Enter" key press event here
			check_spelling(selected_text, selected_langauge);
		}
	};

	const replaceCharacter = (input: String, index: number, replacement: String): String => {
		return input.slice(0, index) + replacement + input.slice(index + replacement.length);
	};

	const clean_input = (input: String): String => {
		if (input.length === 0) {
			return 'Nothing to see here 👀';
		}
		input = input.trim();
		input = input.toLocaleLowerCase();
		let chars_to_clean: String[] = ['.', ',', '!', '?'];
		let words_input: String[] = input.split(' ');
		words_input.forEach((word) => {
			let last_char = word.charAt(word.length - 1);
			if (chars_to_clean.includes(last_char)) {
				word = replaceCharacter(word, word.length - 1, '');
			}
		});
		return words_input.join(' ');
	};

	const check_spelling = (input: String, lang: String): void => {
		let dictionary: Set<String>;
		input = clean_input(input);
		if (lang === 'english') {
			dictionary = english_dictionary;
		} else {
			dictionary = german_dictionary;
		}
		if (input === 'Nothing to see here 👀') {
			wrong_words_finish = 'All correct ✅';
		}
		let words_to_check: String[] = input.split(' ');
		let wrong_words: String[] = [];
		words_to_check.forEach((word) => {
			if (!dictionary.has(word)) {
				wrong_words.push(word);
			}
		});
		if (wrong_words.length === 0) {
			wrong_words_finish = 'All correct ✅';
			selected_text = '';

			if (selected_letter_count === 'yes') {
				wrong_words_finish = 'All correct ✅' + ' count: ' + input.length;
				selected_text = '';
			}
			return;
		}
		if (selected_letter_count === 'yes') {
			wrong_words_finish = wrong_words.join(' ') + ' count: ' + input.length;
			selected_text = '';
			return;
		}
		wrong_words_finish = wrong_words.join(' ');
		selected_text = '';
		return;
	};
</script>

<div on:keydown={handle_keydown} class="container h-full mx-auto flex justify-center items-center">
	<div class="space-y-10 text-center flex flex-col items-center">
		<h2 class="h1">Check that spelling !</h2>
		<!-- Animated Logo -->
		<section class="img-bg" />
		<div class="card p-4 w-full text-token space-y-4">
			<form class="form" />
			<label class="label">
				<span>Langauge</span>
				<select bind:value={selected_langauge} class="select">
					<option value="german">German</option>
					<option value="english">English 󠁧󠁢󠁥󠁮󠁧󠁿</option>
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
					placeholder="Text to check out 🔎"
				/>
			</label>
		</div>

		<!-- / -->
		<div class="flex justify-center space-x-2">
			<button
				on:click={check_spelling(selected_text, selected_langauge)}
				type="button"
				class="btn variant-filled"
			>
				<span>Check spelling</span>
			</button>
			<div class="card p-4 w-full text-token space-y-4">
				<p>{wrong_words_finish}</p>
			</div>
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
	textarea:focus,
	input:focus {
		outline: none;
	}
</style>
