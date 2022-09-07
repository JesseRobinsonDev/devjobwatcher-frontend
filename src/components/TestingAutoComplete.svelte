<script>
	import { onMount } from 'svelte';
	import OutClick from 'svelte-outclick';
	export let tags = [];
	export let placeholder = '';
	export let onChangeTag = () => {};

	let validTags = [];
	export let selectedTags = [];

	let input = '';
	let inputElement;

	let isFocused = false;

	onMount(async () => {
		validTags = [...tags];
	});

	function inputChange() {
		var PATTERN = input.toLowerCase();
		validTags = tags.filter(function (str) {
			return str.includes(PATTERN);
		});
	}

	export function selectTag(tag) {
		if (selectedTags.includes(tag)) {
			selectedTags.splice(selectedTags.indexOf(tag), 1);
			selectedTags = [...selectedTags];
		} else {
			selectedTags = [...selectedTags, tag];
		}
		onChangeTag();
	}

	function clickOutside() {
		isFocused = false;
	}
</script>

<div class="autocomplete-container">
	<input
		bind:value={input}
		bind:this={inputElement}
		on:input={inputChange}
		on:focus={() => {
			isFocused = true;
		}}
		{placeholder}
	/>
	<div class="select-container">
		{#if isFocused}
			<OutClick on:outclick={clickOutside} excludeByDOMNode={[inputElement]}>
				<ul>
					{#each validTags as tag}
						<li on:click={selectTag(tag)} class={selectedTags.includes(tag) ? 'selected-tag' : ''}>
							{tag}
						</li>
					{/each}
				</ul>
			</OutClick>
		{/if}
	</div>
</div>

<style>
	.autocomplete-container {
		width: 100%;
	}

	input {
		width: 100%;
		font-size: 1.15rem;
	}

	input:focus {
		outline: 0;
	}

	.select-container {
		position: relative;
		width: 100%;
	}

	ul {
		width: 100%;
		position: absolute;
		max-height: 16rem;
		padding: 0;
		margin: 0;
		list-style: none;
		overflow-y: scroll;
		display: flex;
		flex-direction: column;
	}

	li {
		padding: 0.05rem;
		font-size: 1.05rem;
		margin: 0;
		font-family: sans-serif;
		color: white;
		display: flex;
		justify-content: center;
		background-color: rgb(40, 40, 40);
		border-bottom: 0.0625rem solid rgb(60, 60, 60);
		transition: all 300ms;
	}

	li:hover {
		background-color: rgb(37, 107, 48);
		cursor: pointer;
	}

	.selected-tag {
		background-color: rgb(37, 107, 48);
	}
</style>
