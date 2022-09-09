<script>
	import 'bulma/css/bulma.css';
	import Button from 'svelma/src/components/Button.svelte';
	import Tag from 'svelma/src/components/Tag/Tag.svelte';

	export let job;
</script>

<div class="job-card">
	<div class="job-info">
		<a href={job.jobLink} class="job-title">{job.jobTitle}</a>
		<span class="job-company">{job.jobCompany}</span>
		<div style="display: flex; flex-direction: row; align-items: center; gap: 0.125rem;">
			<ul style="display: flex; flex-direction: row; gap: 0.125rem; flex-wrap: wrap;">
				{#each job.jobType as type}
					<li><Tag type="is-primary">{type}</Tag></li>
				{/each}
			</ul>
			{#if job.jobRemote == 1}
				<Tag type="is-primary">remote</Tag>
			{/if}
			{#if job.jobSalaryLow}
				<Tag type="is-warning">
					<span>USD {job.jobSalaryLow}</span>
					{#if job.jobSalaryHigh}
						<span>-{job.jobSalaryHigh}</span>
					{/if}
				</Tag>
			{/if}
		</div>
	</div>
	<div class="job-tags">
		<div style="display: flex; flex-direction: row; gap: 0.125rem;">
			<span>
				{#if job.jobLevel}
					<Tag type="is-success">{job.jobLevel}</Tag>
				{/if}
			</span>
			<ul style="display: flex; flex-direction: row; gap: 0.125rem; flex-wrap: wrap;">
				{#each job.jobIndustry as industry}
					<li><Tag type="is-info">{industry}</Tag></li>
				{/each}
			</ul>
		</div>
		<ul style="display: flex; flex-direction: row; gap: 0.125rem; flex-wrap: wrap">
			{#each job.jobTechnologies as technology}
				<li><Tag>{technology}</Tag></li>
			{/each}
		</ul>
	</div>
</div>

<!--
id
jobRemote
jobLink 
-->
<style>
	.job-card {
		min-height: 6rem;
		border-radius: 0.25rem;
		padding: 0.25rem 0.5rem;
		display: flex;
		flex-direction: row;
		gap: 0.5rem;
		background-color: rgb(30, 30, 30);
	}

	.job-info {
		width: 50%;
		display: flex;
		flex-direction: column;
	}

	.job-company {
		color: rgb(130, 130, 130);
		font-size: 0.95rem;
		text-overflow: ellipsis;
		white-space: nowrap;
		overflow: hidden;
	}

	.job-title {
		color: rgb(220, 220, 220);
		font-size: 1.1rem;
		text-overflow: ellipsis;
		white-space: nowrap;
		overflow: hidden;
		transition: all 300ms;
	}

	.job-title:hover {
		color: rgb(44, 169, 46);
	}

	.job-tags {
		width: 50%;
		height: 100%;
		display: flex;
		flex-direction: column;
		justify-content: center;
		gap: 0.125rem;
	}

	@media (max-width: 768px) {
		.job-card {
			flex-direction: column;
		}
		.job-info {
			width: 100%;
		}
		.job-tags {
			width: 100%;
		}
	}
</style>
