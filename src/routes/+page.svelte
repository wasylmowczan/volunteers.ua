<script lang="ts">
	import Globe from '$lib/components/custom/Globe.svelte';
</script>

<section class="flex flex-col items-center justify-center px-4 py-4">
	<h1 class="scroll-m-20 px-4 py-4 text-3xl font-extrabold tracking-tight lg:text-3xl">
		<span class="text-blue-500">Volunteer.ua</span>
		<span>the place where volunteers meet to help Ukraine</span>
	</h1>
	<div class="flex w-full items-center justify-center px-4 py-4">
		<div
			class="relative flex h-[1000px] w-full items-center justify-center overflow-hidden rounded-lg px-40 py-40"
		>
			<Globe />
			<div class="pointer-events-none absolute inset-0 h-full w-full" />
		</div>
	</div>
</section>
