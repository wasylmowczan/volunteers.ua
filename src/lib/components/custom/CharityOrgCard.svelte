<!-- src/routes/charities/CharityCard.svelte -->
<script lang="ts">
	import { Button } from '$lib/components/ui/button/index.js';
	import * as Card from '$lib/components/ui/card/index.js';
	import { Badge } from '$lib/components/ui/badge/index.js';
	import { ScrollArea } from '$lib/components/ui/scroll-area/index.js';

	export let logo: string;
	export let name: string;
	export let description: string;
	export let links: { label: string; url: string }[];
	export let badges: string[];
	export let buttonLink: string;
	export let projects: { name: string; url: string }[];

	let logoSrc = logo;
</script>

<Card.Root class="h-full w-full max-w-[650px]">
	<Card.Header>
		<Card.Title>
			<div class="flex items-center gap-2">
				<img src={logoSrc} alt="Logo" class="h-10 w-10" />
				<span>{name}</span>
			</div>
		</Card.Title>
		<Card.Description class="max-h-[100px] overflow-hidden overflow-y-auto">
			{description}
		</Card.Description>
	</Card.Header>
	<Card.Content class="flex flex-col gap-4">
		<Card.Description>
			<ScrollArea class="w-full whitespace-nowrap rounded-md border" orientation="horizontal">
				<div class="flex w-max space-x-4 p-4">
					{#each projects as project, index}
						<div class="flex items-center gap-2">
							<a href={project.url} target="_blank" class="text-blue-500 hover:text-blue-700"
								>{project.name}</a
							>
						</div>
					{/each}
				</div>
			</ScrollArea>
		</Card.Description>
		<div class="flex gap-2">
			{#each badges as badge (badge)}
				<Badge>{badge}</Badge>
			{/each}
		</div>
		{#each links as link}
			<div class="flex items-center gap-2">
				<a href={link.url} target="_blank" class="text-blue-500 hover:text-blue-700">{link.label}</a
				>
			</div>
		{/each}
	</Card.Content>
	<Card.Footer class="flex justify-center">
		<Button href={buttonLink} target="_blank">Visit org website!</Button>
	</Card.Footer>
</Card.Root>
