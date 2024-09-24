<script lang="ts">
	// Importing components and defining charities array
	import CharityOrgCard from '$lib/components/custom/CharityOrgCard.svelte';
	import { Button } from '@/components/ui/button';
	import Input from '@/components/ui/input/input.svelte';

	const charities = [
		{
			logo: '/logos/prytula.svg',
			name: 'Serhiy Prytula Charity Foundation',
			description:
				"Since 2014, Serhiy Prytula has been involved in ivolunteering to help the Ukrainian army as an individual. In 2020, he and his team created a charitable foundation to provide assistance to medical workers, people with disabilities, and to promote education and inclusion. After the start of russia's full-scale invasion, resistance to the enemy became the main task of the entire country. ",
			projects: [
				{
					name: '🏗️ Ongoing projects',
					url: 'https://prytulafoundation.org/en/about/projects'
				}
			],
			links: [
				{
					label: '🪖 Help the army',
					url: 'https://prytulafoundation.org/en/help-army'
				},
				{
					label: '📦 Help civilians',
					url: 'https://prytulafoundation.org/en/humanitarian-help'
				},
				{
					label: '💸 Support foundation',
					url: 'https://prytulafoundation.org/support-foundation'
				}
			],
			badges: ['Army', 'Civilians', 'Medical'],
			buttonLink: 'https://prytulafoundation.org/'
		},
		{
			logo: '/logos/come-back-alive.svg',
			name: 'Come Back Alive',
			description:
				'The Come Back Alive Foundation has been operational since 2014. Our primary objective is to enhance the effectiveness of the Ukrainian Defense Forces, save the lives of our servicemen, and systematically counteract the enemy. Since 2014 (as of April 2024), the Foundation has raised over 356 million dollars for the needs of the Defense Forces of Ukraine. We maintain transparent financial reporting, allowing for easy tracking of every donation and purchase. In 2018, the team of the Come Back Alive Foundation expanded its work and added first the analytical and then the veteran’s directions. Over time, they became departments and formed the NGO ‘Come Back Alive!’. In 2024, the NGO ‘Come Back Alive!’ transformed into the CBA Initiatives Center. The Center’s work takes place in a public environment that complements the activities of the charitable foundation. Currently, the Center has two main areas of activity: analytical and veterans.',
			projects: [
				{
					name: '🏗️ All active projects',
					url: 'https://savelife.in.ua/en/projects/status/active/'
				}
			],
			links: [
				{
					label: '🪖 Help the army',
					url: 'https://savelife.in.ua/donate/#donate-army-card-once'
				},
				{
					label: '💸 Support foundation',
					url: 'https://savelife.in.ua/en/donate-en/#donate-fund-card-once'
				}
			],
			badges: ['Army', 'Civilians', 'Veterans'],
			buttonLink: 'https://savelife.in.ua/'
		}
	];

	// Variables to manage filtering and search
	let activeFilter: string | null = null;
	let filteredCharities = charities;
	let searchQuery: string = '';

	// Function to filter charities by category
	function filterCharities(category: string) {
		activeFilter = category;
		if (category === 'All') {
			filteredCharities = charities;
		} else {
			filteredCharities = charities.filter((charity) => charity.badges.includes(category));
		}
	}

	// Function to filter charities by name
	function filterByName(event: InputEvent) {
		searchQuery = (event.target as HTMLInputElement).value;
		filteredCharities = charities.filter((charity) =>
			charity.name.toLowerCase().includes(searchQuery.toLowerCase())
		);
	}
</script>

<div class="flex justify-center gap-2 pt-4">
	<Button
		variant={activeFilter === 'Army' ? 'outline' : 'outline'}
		on:click={() => filterCharities('Army')}>Army</Button
	>
	<Button
		variant={activeFilter === 'Civilians' ? 'default' : 'outline'}
		on:click={() => filterCharities('Civilians')}>Civilians</Button
	>
	<Button
		variant={activeFilter === 'Medical' ? 'default' : 'outline'}
		on:click={() => filterCharities('Medical')}>Medical</Button
	>
	<Button
		variant={activeFilter === 'Veterans' ? 'default' : 'outline'}
		on:click={() => filterCharities('Veterans')}>Veterans</Button
	>
	<Button
		variant={activeFilter === 'All' ? 'default' : 'outline'}
		on:click={() => filterCharities('All')}>All</Button
	>
</div>

<div class="flex justify-center py-4">
	<Input class="w-1/2 max-w-sm" placeholder="Search by name" on:input={filterByName} />
</div>

{#if activeFilter}
	<p class="text-center text-sm text-muted-foreground">
		Showing Charities for: <strong>{activeFilter}</strong>
	</p>
{:else}
	<p class="text-center text-sm text-muted-foreground">Showing <strong>All Charities</strong></p>
{/if}

<div class="grid grid-cols-1 justify-items-center p-2 pt-4 md:grid-cols-2">
	{#each filteredCharities as charity}
		<div class="group relative">
			<CharityOrgCard
				logo={charity.logo}
				name={charity.name}
				description={charity.description}
				projects={charity.projects}
				links={charity.links}
				badges={charity.badges}
				buttonLink={charity.buttonLink}
			/>
		</div>
	{/each}
</div>
