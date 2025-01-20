<script>
	import DaysVisualizer from '../components/DaysVisualizer.svelte';

	const structuredData = {
		'@context': 'https://schema.org',
		'@type': 'WebApplication',
		name: 'Year Progress Visualizer',
		description: 'Visualize the days left in the year with an interactive dot grid calendar.',
		applicationCategory: 'Utility',
		operatingSystem: 'Any'
	};
</script>

<svelte:head>
	<script type="application/ld+json">
		{JSON.stringify(structuredData)}
	</script>
</svelte:head>

<main class="flex min-h-screen items-center justify-center bg-black p-4">
	<article class="w-full">
		<h1 class="sr-only">Year Progress Visualizer</h1>
		<DaysVisualizer />
	</article>
</main>
