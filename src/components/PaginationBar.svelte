<script>
	export let currentPage = 1;
	export let onChange = () => {};

	function updatePage(increment) {
		currentPage += increment;
		onChange();
	}
</script>

<nav>
	{#if currentPage > 1}<button on:click={() => updatePage(-1)}>{'<'}</button>{/if}
	{#if currentPage > 3}<button on:click={() => updatePage(-3)}>{currentPage - 3}</button>{/if}
	{#if currentPage > 2}<button on:click={() => updatePage(-2)}>{currentPage - 2}</button>{/if}
	{#if currentPage > 1}<button on:click={() => updatePage(-1)}>{currentPage - 1}</button>{/if}
	<button class="current-page" on:click={() => updatePage(0)}>{currentPage}</button>
	<button on:click={() => updatePage(1)}>{currentPage + 1}</button>
	<button on:click={() => updatePage(2)}>{currentPage + 2}</button>
	<button on:click={() => updatePage(3)}>{currentPage + 3}</button>
	{#if currentPage < 4}<button on:click={() => updatePage(4)}>{currentPage + 4}</button>{/if}
	{#if currentPage < 3}<button on:click={() => updatePage(5)}>{currentPage + 5}</button>{/if}
	{#if currentPage < 2}<button on:click={() => updatePage(6)}>{currentPage + 6}</button>{/if}
	<button on:click={() => updatePage(1)}>{'>'}</button>
</nav>

<style>
	nav {
		display: flex;
		flex-direction: row;
		gap: 0.175rem;
	}

	button {
		outline: 0;
		border: 0;
		margin: 0;
		padding: 0.2rem 0.6rem;
		font-size: 1.15rem;
		background-color: transparent;
		color: white;
		border-radius: 0.125rem;
		transition: all 300ms;
	}

	button:hover {
		cursor: pointer;
		background-color: rgba(25, 69, 200, 0.65);
	}

	.current-page {
		background-color: rgb(25, 69, 200);
	}
</style>
