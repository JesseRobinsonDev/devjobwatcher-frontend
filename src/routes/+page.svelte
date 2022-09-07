<script>
	import { onMount } from 'svelte';
	import JobCard from '../components/JobCard.svelte';

	let technologies = [
		'rest',
		'graphql',
		'npm',
		'yarn',
		'phoenix',
		'spring',
		'symfony',
		'ruby on rails',
		'laravel',
		'asp.net',
		'django',
		'flask',
		'flutter',
		'react native',
		'svelte',
		'angular',
		'vue',
		'react',
		'expressjs',
		'nodejs',
		'kubernetes',
		'docker',
		'git',
		'azure',
		'google cloud services',
		'aws',
		'cassandradb',
		'dynamodb',
		'oracle',
		'firebase',
		'mariadb',
		'sqlite',
		'redis',
		'mongodb',
		'postgresql',
		'mysql',
		'jquery',
		'sass',
		'css',
		'html',
		'sql',
		'typescript',
		'swift',
		'scala',
		'ruby',
		'python',
		'php',
		'perl',
		'matlab',
		'kotlin',
		'julia',
		'javascript',
		'java',
		'haskell',
		'golang',
		'f#',
		'elixir',
		'dart',
		'carbon',
		'c#'
	].reverse();
	let levels = ['internship', 'junior', 'intermediate', 'senior'];
	let types = ['fulltime', 'parttime'];
	let industries = [
		'frontend',
		'backend',
		'fullstack',
		'machine learning',
		'data science',
		'devops'
	];

	let selectedTechnologies = [];
	let selectedLevels = [];
	let selectedTypes = [];
	let selectedIndustries = [];

	let mounted = false;

	import axios from 'axios';
	import TestingAutoComplete from '../components/TestingAutoComplete.svelte';
	import 'bulma/css/bulma.css';
	import Tag from 'svelma/src/components/Tag/Tag.svelte';

	let jobListings = [];

	onMount(async () => {
		axios
			.get(`${import.meta.env.VITE_API_URL}/jobs?limit=50`)
			.catch((error) => {})
			.then((response) => {
				if (response !== undefined) {
					mounted = true;
					jobListings = [...response.data];
				}
			});
	});

	function updateTags() {
		if (!mounted) {
			return;
		}
		axios
			.get(
				`${import.meta.env.VITE_API_URL}/jobs?limit=50${
					selectedTechnologies.length > 0 ? `&technologies=${selectedTechnologies.join(',')}` : ''
				}${selectedLevels.length > 0 ? `&levels=${selectedLevels.join(',')}` : ''}${
					selectedTypes.length > 0 ? `&types=${selectedTypes.join(',')}` : ''
				}${selectedIndustries.length > 0 ? `&industries=${selectedIndustries.join(',')}` : ''}`
			)
			.catch((error) => {})
			.then((response) => {
				if (response !== undefined) {
					jobListings = [...response.data];
				}
			});
	}

	let removeTechnologyTag;
	let removeLevelTag;
	let removeTypeTag;
	let removeIndustryTag;

	function closeTag(tag, type) {
		switch (type) {
			case 'technology':
				removeTechnologyTag(tag);
				return;
			case 'level':
				removeLevelTag(tag);
				return;
			case 'type':
				removeTypeTag(tag);
				return;
			case 'industry':
				removeIndustryTag(tag);
				return;
		}
	}
</script>

<div
	style="width: 100%; height: 100%; display: flex; flex-direction: column; align-items: center; gap: 1rem;"
>
	<div class="filters-container">
		<input />
		<div class="filter-tags-container">
			{#if selectedTechnologies.length > 0}
				<div class="filter-tag-list">
					<Tag type="is-dark">Skills:</Tag>
					{#each selectedTechnologies as tag}
						<Tag type="is-dark" closable on:close={closeTag(tag, 'technology')}>{tag}</Tag>
					{/each}
				</div>
			{/if}
			{#if selectedLevels.length > 0}
				<div class="filter-tag-list">
					<Tag type="is-dark">Levels:</Tag>
					{#each selectedLevels as tag}
						<Tag type="is-dark" closable on:close={closeTag(tag, 'level')}>{tag}</Tag>
					{/each}
				</div>
			{/if}
			{#if selectedTypes.length > 0}
				<div class="filter-tag-list">
					<Tag type="is-dark">Types:</Tag>
					{#each selectedTypes as tag}
						<Tag type="is-dark" closable on:close={closeTag(tag, 'type')}>{tag}</Tag>
					{/each}
				</div>
			{/if}
			{#if selectedIndustries.length > 0}
				<div class="filter-tag-list">
					<Tag type="is-dark">Industries:</Tag>
					{#each selectedIndustries as tag}
						<Tag type="is-dark" closable on:close={closeTag(tag, 'industry')}>{tag}</Tag>
					{/each}
				</div>
			{/if}
		</div>
		<div class="filter-inputs-container">
			<div class="filter-input-container">
				<TestingAutoComplete
					bind:selectTag={removeTechnologyTag}
					onChangeTag={updateTags}
					tags={technologies}
					bind:selectedTags={selectedTechnologies}
					placeholder="Skills"
				/>
			</div>
			<div class="filter-input-container">
				<TestingAutoComplete
					bind:selectTag={removeLevelTag}
					onChangeTag={updateTags}
					tags={levels}
					bind:selectedTags={selectedLevels}
					placeholder="Level"
				/>
			</div>
			<div class="filter-input-container">
				<TestingAutoComplete
					bind:selectTag={removeTypeTag}
					onChangeTag={updateTags}
					tags={types}
					bind:selectedTags={selectedTypes}
					placeholder="Type"
				/>
			</div>
			<div class="filter-input-container">
				<TestingAutoComplete
					bind:selectTag={removeIndustryTag}
					onChangeTag={updateTags}
					tags={industries}
					bind:selectedTags={selectedIndustries}
					placeholder="Industry"
				/>
			</div>
		</div>
	</div>
	<ul class="jobs-container">
		{#each jobListings as job}
			<JobCard {job} />
		{/each}
	</ul>
</div>

<style>
	.filters-container {
		display: flex;
		flex-direction: column;
		width: 50%;
	}

	.filter-tags-container {
		display: flex;
		flex-direction: column;
		gap: 0.125rem;
	}

	.filter-tag-list {
		display: flex;
		flex-direction: row;
		flex-wrap: wrap;
		gap: 0.125rem;
		border-bottom: 0.125rem solid rgb(40, 40, 40);
		padding: 0.25rem;
	}

	.filter-inputs-container {
		display: grid;
		grid-template-columns: repeat(4, 1fr);
	}

	.jobs-container {
		width: 50%;
		display: flex;
		flex-direction: column;
		gap: 0.25rem;
	}

	@media (max-width: 1152px) {
		.filter-inputs-container {
			display: grid;
			grid-template-columns: repeat(2, 1fr);
		}
		.filters-container {
			width: 65%;
		}
		.jobs-container {
			width: 65%;
		}
	}

	@media (max-width: 896px) {
		.filter-inputs-container {
			display: grid;
			grid-template-columns: repeat(1, 1fr);
		}
		.filters-container {
			width: 80%;
		}
		.jobs-container {
			width: 80%;
		}
	}
</style>
