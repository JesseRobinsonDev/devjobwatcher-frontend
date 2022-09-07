<script>
	import { onMount } from 'svelte';
	import JobCard from '../components/JobCard.svelte';

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
</script>
<div style="width: 100%; height: 100%; display: flex; justify-content: center">
	<ul style="width: 50%; display: flex; flex-direction: column; gap: 0.25rem;">
		{#each jobListings as job}
			<JobCard {job} />
		{/each}
	</ul>
</div>
