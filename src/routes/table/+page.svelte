<script lang="ts">
	import type { Data } from 'src/types/Data';
	import { data } from './data';

	let selectedName = '';
	let filteredItemsByName: Data[] = data;
	$: if (selectedName) getItemByName();

	const getItemByName = () => {
		if (selectedName === 'all') return (filteredItemsByName = data);
		return (filteredItemsByName = data.filter((item) => {
			return item.name === selectedName;
		}));
	};

	let ascending = false;

	const sort = () => {
		filteredItemsByName = filteredItemsByName.sort();
		if (!ascending) filteredItemsByName = filteredItemsByName.reverse();
	};
</script>

<h1>Table</h1>
<search>
	<section>
		<select class="filter" name="filter" id="filter" bind:value={selectedName}>
			<option disabled selected value="">Filter by Name.</option>
			<option value="all">See all</option>
			{#each data as item}
				<option value={item.name}>{item.name}</option>
			{/each}
		</select>
	</section>
</search>
<table class="table">
	<thead>
		<tr>
			<th>ID</th>
			<th class="name_sort" on:click={sort}> &#8645 Name </th>
			<th>Data</th>
		</tr>
	</thead>
	<tbody>
		{#each filteredItemsByName as item}
			<tr>
				<td>{item.id}</td>
				<td>{item.name}</td>
				<td>
					{#each Object.entries(item.data) as [title, value]}
						{title}: {value}<br />
					{/each}
				</td>
			</tr>
		{/each}
	</tbody>
</table>

<style>
	.table {
		border-collapse: collapse;
		margin: 0 0 0.5rem 0;
		padding: 0.5rem;
		background-color: white;
		border-radius: 8px;
		filter: drop-shadow(2px 4px 6px rgba(0, 0, 0, 0.1));
	}

	.table thead {
		border-bottom: 1px solid rgba(0, 0, 0, 0.1);
		padding: 5rem;
	}

	.table th,
	.table td {
		padding: 12px 15px;
		text-align: center;
	}
	.table thead tr {
		color: #ba6063;
	}

	.name_sort {
		display: flex;
		align-items: center;
		justify-content: center;
		gap: 0.3rem;
		cursor: pointer;
	}
</style>
