<script lang="ts">
	import '../app.css';
	import Menu from 'lucide-svelte/icons/menu';
	import { Button } from '$lib/components/ui/button/index.js';
	import * as Sheet from '$lib/components/ui/sheet/index.js';
	import * as Card from '$lib/components/ui/card/index.js';
	import { BookHeart, Bird, Github, ScrollText } from 'lucide-svelte';
	import { ModeWatcher } from 'mode-watcher';

	import Sun from 'lucide-svelte/icons/sun';
	import Moon from 'lucide-svelte/icons/moon';
	import { toggleMode } from 'mode-watcher';

	import { onMount } from 'svelte';
	import { page } from '$app/stores';

	const menu = [{ icon: BookHeart, text: 'Charities', href: '/charities' }];

	let activeHref = '';

	// Set the activeHref based on the current page
	onMount(() => {
		const unsubscribe = page.subscribe(($page) => {
			activeHref = $page.url.pathname; // Get the current path
		});
		return () => unsubscribe();
	});
</script>

<ModeWatcher />
<!-- Layout for all pages -->

<div class="grid min-h-screen w-full md:grid-cols-[240px_1fr] lg:grid-cols-[240px_1fr]">
	<div class="hidden border-r bg-muted/40 md:block">
		<div class="sticky top-0 flex h-screen flex-col gap-2">
			<div class="flex h-14 items-center border-b px-4 lg:h-[60px] lg:px-6">
				<a href="/" class="flex items-center gap-2 text-xl font-semibold">
					<Bird class="h-10 w-8" />
					<span class="">Volunteers.ua</span>
				</a>
			</div>

			{#each menu as item}
				<div class="w-full flex-1 py-4">
					<nav class="grid px-2 text-lg font-medium lg:px-4">
						<a href={item.href} class="flex items-center gap-3 rounded-lg px-3 py-2">
							<svelte:component this={item.icon} class="h-4 w-4" />
							{#if activeHref === item.href}
								{item.text} 👉
							{:else}
								{item.text}
							{/if}
						</a>
					</nav>
				</div>
			{/each}

			<!-- Bottom menu -->
			<Card.Header>
				<Card.Title class="text-sm">Want to help?</Card.Title>
				<Card.Description class="text-xs">
					Join us at GitHub and help us improve Volunteers.ua together!
				</Card.Description>
			</Card.Header>
			<Card.Content>
				<Button
					variant="ghost"
					size="sm"
					class="flex w-full justify-start gap-2"
					href="https://github.com/wasylmowczan/volunteers.ua"
					target="_blank"
				>
					<Github class="h-4 w-4" />
					GitHub
				</Button>
				<Button variant="ghost" size="sm" class="flex w-full justify-start gap-2" href="/changelog">
					<ScrollText class="h-4 w-4" />
					Changelog
				</Button>
			</Card.Content>
		</div>
	</div>

	<!-- Mobile navigation -->
	<div class="flex flex-col">
		<header
			class="sticky top-0 z-50 flex h-14 items-center gap-4 border-b bg-muted/40 px-4 lg:h-[60px] lg:px-6"
		>
			<Sheet.Root>
				<Sheet.Trigger asChild let:builder>
					<Button variant="outline" size="icon" class="shrink-0 md:hidden" builders={[builder]}>
						<Menu class="h-5 w-5" />
						<span class="sr-only">Toggle navigation menu</span>
					</Button>
				</Sheet.Trigger>
				<Sheet.Content side="left" class="flex flex-col">
					<nav class="grid gap-2 text-lg font-medium lg:px-6">
						<a href="/" class="flex items-center gap-2 border-b text-lg font-semibold">
							<Bird class="h-6 w-6" />
							<span>Volunteers.ua</span>
						</a>
						{#each menu as item}
							<a
								href={item.href}
								class="mx-[-0.65rem] flex items-center gap-4 rounded-xl px-3 py-2 text-muted-foreground hover:text-foreground"
							>
								<svelte:component this={item.icon} class="h-5 w-5" />
								{item.text}
							</a>
						{/each}
					</nav>
					<div class="mt-auto"></div>
				</Sheet.Content>
			</Sheet.Root>

			<div class="w-full flex-1"></div>
			<Button on:click={toggleMode} variant="outline" size="icon">
				<Sun
					class="h-[1.2rem] w-[1.2rem] rotate-0 scale-100 transition-all dark:-rotate-90 dark:scale-0"
				/>
				<Moon
					class="absolute h-[1.2rem] w-[1.2rem] rotate-90 scale-0 transition-all dark:rotate-0 dark:scale-100"
				/>
				<span class="sr-only">Toggle theme</span>
			</Button>
		</header>

		<!-- Slot for page content -->
		<slot></slot>
	</div>
</div>
