<script lang="ts">
	import type { PageData } from './$types';

	import Card from './Card.svelte';
	import Entry from './Entry.svelte';
	import LukaPonaEntry from './LukaPonaEntry.svelte';
	import Navbar from './Navbar.svelte';
	import Filter from './Filter.svelte';

	import search from '$lib/components/search.js';

	export let data: PageData;

	let lightmode = false;
	$: if (lightmode) {
		document.documentElement.classList.add('lightmode');
	} else {
		document.documentElement.classList.remove('lightmode');
	}

	const dictionary = data.data;
	const languages = data.languages;

	let query = '';

	let selected_language = 'en';

	let selected_view = 'basic';
	let categories = [
		{ name: 'core', checked: true },
		{ name: 'widespread', checked: true },
		{ name: 'common', checked: false },
		{ name: 'uncommon', checked: false },
		{ name: 'rare', checked: false },
		{ name: 'obscure', checked: false }
	];

	$: categories_short = Object.fromEntries(
		Array.from(categories, (item) => {
			return [item.name, item.checked];
		})
	);

	$: sorted_filtered_dictionary = search(dictionary, query, categories_short);
</script>

<div class="app">
	<Navbar bind:query bind:lightmode bind:selected_language bind:selected_view {languages} />
	<a id="survey" href="https://linku.la/wile/">
		2023 Word Survey: Let&nbsp;us&nbsp;know&nbsp;what&nbsp;words&nbsp;you&nbsp;use!
	</a>
	<Filter bind:categories />
	<div class={selected_view == 'grid' ? 'view_grid' : 'view_basic'}>
		{#each Object.entries(sorted_filtered_dictionary) as [key, word] (key)}
			{#if selected_view == 'basic'}
				<Entry {word} {selected_language} />
			{:else if selected_view == 'grid'}
				<Card {word} {selected_language} />
			{:else}
				<LukaPonaEntry {word} {selected_language} />
			{/if}
		{/each}
	</div>
</div>

<style>
	@font-face {
		font-family: 'sitelen seli kiwen';
		font-style: normal;
		font-weight: 400;
		src: url(https://raw.githubusercontent.com/lipu-linku/nasin-sitelen/main/sitelenselikiwenasuki.ttf);
	}
	.app {
		margin: 0 auto;
		padding: 0;
	}
	.view_basic {
		display: block;
		margin: auto;
		padding: 0 10px;
		max-width: 840px;
	}
	.view_grid {
		display: grid;
		grid-template-columns: repeat(auto-fit, minmax(180px, 1fr));
	}
	#survey {
		display: block;
		text-align: center;
		font-size: 1.2em;
		font-weight: bold;
		margin-top: 1rem;
	}
</style>
