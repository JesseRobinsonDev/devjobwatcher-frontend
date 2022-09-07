<script>
	import { onMount } from 'svelte';
	import JobCard from '../components/JobCard.svelte';
	import AutoComplete from '../components/AutoComplete.svelte';

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

	let jobListings = [];

	onMount(async () => {
		console.log('mounting');
		axios
			.get(`${import.meta.env.VITE_API_URL}/jobs?limit=50`)
			.catch((error) => {
				console.log(error);
			})
			.then((response) => {
				if (response !== undefined) {
					mounted = true;
					jobListings = [...response.data];
					console.log(response);
				}
			});
	});

	function updateTags() {
		if (!mounted) {
			return;
		}
		console.log('Update Tags');
		axios
			.get(
				`${import.meta.env.VITE_API_URL}/jobs?limit=50${
					selectedTechnologies.length > 0 ? `&technologies=${selectedTechnologies.join(',')}` : ''
				}${selectedLevels.length > 0 ? `&levels=${selectedLevels.join(',')}` : ''}${
					selectedTypes.length > 0 ? `&types=${selectedTypes.join(',')}` : ''
				}${selectedIndustries.length > 0 ? `&industries=${selectedIndustries.join(',')}` : ''}`
			)
			.catch((error) => {
				console.log(error);
			})
			.then((response) => {
				if (response !== undefined) {
					jobListings = [...response.data];
					console.log(response);
				}
			});
	}
</script>

<div style="width: 50%; display: flex; flex-direction: row; gap: 0.125rem;">
	<AutoComplete
		onChange={updateTags}
		tags={technologies}
		bind:selectedTags={selectedTechnologies}
		placeholder="Technologies"
	/>
	<AutoComplete
		onChange={updateTags}
		tags={levels}
		bind:selectedTags={selectedLevels}
		placeholder="Level"
	/>
	<AutoComplete
		onChange={updateTags}
		tags={types}
		bind:selectedTags={selectedTypes}
		placeholder="Type"
	/>
	<AutoComplete
		onChange={updateTags}
		tags={industries}
		bind:selectedTags={selectedIndustries}
		placeholder="Industry"
	/>
</div>
<div style="width: 100%; height: 100%; display: flex; justify-content: center">
	<ul style="width: 50%; display: flex; flex-direction: column; gap: 0.25rem;">
		{#each jobListings as job}
			<JobCard {job} />
		{/each}
	</ul>
</div>
