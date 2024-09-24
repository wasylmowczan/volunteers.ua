<script lang="ts">
	import { onMount } from 'svelte';
	import createGlobe from 'cobe';
	import { spring } from 'svelte/motion';
	import { cn } from '$lib/utils';

	let x = spring(0, {
		stiffness: 0.04,
		damping: 0.4,
		precision: 0.005
	});

	let className = '';
	export { className as class };
	let pointerInteracting: any = null;
	let pointerInteractionMovement = 0;
	let canvas: HTMLCanvasElement;

	let phi = 0;
	let width = 0;
	$: console.log(width, 'X');
	let onResize = () => {
		width = canvas.offsetWidth;
	};

	let onRender = (state: any) => {
		if (!pointerInteracting) {
			phi += 0.005;
		}
		state.phi = phi + $x;
		state.width = width * 2;
		state.height = width * 2;
	};

	onMount(() => {
		// Adds the resize event listener when the component is mounted
		window.addEventListener('resize', onResize);
		onResize();

		// Initializes the globe with specific options
		const globe = createGlobe(canvas, {
			devicePixelRatio: 2,
			width: width,
			height: width,
			phi: 0,
			theta: 0.3,
			dark: 1,
			diffuse: 0.4, // 1.2
			mapSamples: 16000,
			mapBrightness: 1.5, // 6
			baseColor: [0.3, 0.3, 0.3],
			markerColor: [251 / 255, 100 / 255, 21 / 255],
			glowColor: [1, 1, 1],
			markers: [{ location: [48.3794, 31.1656], size: 0.03 }], // Ukraine
			onRender: (state) => {
				if (!pointerInteracting) {
					// Called on every animation frame.
					// `state` will be an empty object, return updated params.
					phi += 0.009;
				}
				state.phi = phi + $x;

				// phi += 0.01;
			},
			onRender: onRender
		});

		// Removes the resize event listener when the component is unmounted to prevent memory leaks
		return () => {
			window.removeEventListener('resize', onResize);
		};
	});
</script>

<main class={cn('absolute inset-2 mx-auto aspect-[1/1.2] w-full max-w-[600px]', className)}>
	<canvas
		class="h-full w-full [contain:layout_paint_size]"
		bind:this={canvas}
		on:pointerdown={(e) => {
			pointerInteracting = e.clientX - pointerInteractionMovement;
			canvas.style.cursor = 'grabbing';
		}}
		on:pointerup={() => {
			pointerInteracting = null;
			canvas.style.cursor = 'grab';
		}}
		on:pointerout={() => {
			pointerInteracting = null;
			canvas.style.cursor = 'grab';
		}}
		on:mousemove={(e) => {
			if (pointerInteracting !== null) {
				console.log('working');
				const delta = e.clientX - pointerInteracting;
				pointerInteractionMovement = delta;
				x.set(delta / 200);
			}
		}}
	/>
</main>
