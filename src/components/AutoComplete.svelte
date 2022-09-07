<script>
	import AutoComplete from 'simple-svelte-autocomplete';
	import 'bulma/css/bulma.css';

	export let onChange;
	export let tags = [];
	export let selectedTags;
	export let placeholder;
</script>

<AutoComplete
	multiple="true"
	items={tags}
	bind:selectedItem={selectedTags}
	{onChange}
	{placeholder}
/>
