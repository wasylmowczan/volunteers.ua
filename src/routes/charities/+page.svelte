<script lang="ts">
	// Importing components and defining charities array
	import CharityOrgCard from '$lib/components/custom/CharityOrgCard.svelte';
	import Badge from '@/components/ui/badge/badge.svelte';
	import { Button } from '@/components/ui/button';
	import Input from '@/components/ui/input/input.svelte';

	enum Badges {
		Army = 'Army',
		Civilians = 'Civilians',
		Medical = 'Medical',
		Infrastructure = 'Infrastructure',
		EducationAndScience = 'Education and Science',
		Veterans = 'Veterans',
		Relief = 'Relief',
		Advocacy = 'Advocacy'
	}

	const charities = [
		{
			logo: '/logos/united24.svg',
			name: 'United24',
			description:
				'United24 is a Ukrainian government-run platform launched on 5 May 2022 to raise money for Ukraine in the Russo-Ukrainian War. The funds are transferred to the accounts of the National Bank of Ukraine and assigned to the relevant ministries: the Ministry of Defense, the Ministry of Healthcare, and the Ministry of Infrastructure. The reports on the platform are updated daily.',
			projects: [
				{
					name: '🏗️ All projects',
					url: 'https://u24.gov.ua/projects'
				}
			],
			links: [
				{
					label: '🪖 Help the army',
					url: 'https://u24.gov.ua/'
				},
				{
					label: '📦 Help civilians',
					url: 'https://u24.gov.ua/'
				},
				{
					label: '💊 Medical aid',
					url: 'https://u24.gov.ua/'
				},
				{
					label: '🏗️ Rebuild Ukraine',
					url: 'https://u24.gov.ua/'
				},
				{
					label: '🎒 Education and Science',
					url: 'https://u24.gov.ua/'
				}
			],
			badges: [
				Badges.Army,
				Badges.Civilians,
				Badges.Medical,
				Badges.Infrastructure,
				Badges.EducationAndScience
			],
			buttonLink: 'https://u24.gov.ua/'
		},
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
			badges: [Badges.Army, Badges.Civilians, Badges.Medical],
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
			badges: [Badges.Army, Badges.Civilians, Badges.Veterans],
			buttonLink: 'https://savelife.in.ua/'
		},
		{
			logo: '/logos/razom.png',
			name: 'Razom for Ukraine',
			description:
				'Razom, which means “together” in Ukrainian, believes deeply in the enormous potential of dedicated volunteers around the world united by a single goal: to unlock the potential of Ukraine. Razom works towards that mission by creating spaces where people meet, partner and do. Razom was born out of the Revolution of Dignity in 2014 when millions of people worked together and risked their lives to build a pathway to a better future for Ukraine.  Those who were unable to be in Ukraine during this time, wanted to do their part to help the movement as best they could from abroad. Many sent funds and supplies to sustain the community built on the Maidan through the winter, but they also took to the streets in their own cities to raise awareness and amplify voices from Ukraine in the West.',
			projects: [
				{
					name: '🏗️ All programs',
					url: 'https://www.razomforukraine.org/programs/'
				}
			],
			links: [
				{
					label: '💸 Donate to foundation',
					url: 'https://www.razomforukraine.org/donate/'
				}
			],
			badges: [Badges.Army, Badges.Medical, Badges.Relief, Badges.Advocacy],
			buttonLink: 'https://www.razomforukraine.org/'
		}
	];

	// Variables to manage filtering and search
	let activeFilter: Badges | null = null;
	let filteredCharities = charities;
	let searchQuery: string = '';

	// Function to filter charities by category
	function filterCharities(category: Badges | null) {
		activeFilter = category;
		if (category) {
			filteredCharities = charities.filter((charity) => charity.badges.includes(category));
		} else {
			filteredCharities = charities;
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
	{#each Object.values(Badges) as badge}
		<Button
			variant={activeFilter === badge ? 'default' : 'outline'}
			on:click={() => filterCharities(badge)}>{badge}</Button
		>
	{/each}
	<Button
		variant={activeFilter === null ? 'default' : 'outline'}
		on:click={() => filterCharities(null)}>All</Button
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

<div
	class="grid grid-cols-1 justify-items-center gap-4 p-4 md:grid-cols-2 lg:grid-cols-2 xl:grid-cols-3 2xl:grid-cols-3"
>
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
