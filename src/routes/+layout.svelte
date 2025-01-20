<script lang="ts">
	import '../app.css';
	let { children } = $props();
</script>

<svelte:head>
	<title>Year Progress Visualizer</title>
	<meta
		name="description"
		content="Visualize the days left in the year with an interactive dot grid calendar."
	/>

	<!-- Open Graph / Facebook -->
	<meta property="og:type" content="website" />
	<!-- <meta property="og:url" content="https://yourwebsite.com/" /> -->
	<meta property="og:title" content="Year Progress Visualizer" />
	<meta
		property="og:description"
		content="Visualize the days left in the year with an interactive dot grid calendar."
	/>
	<!-- <meta property="og:image" content="https://yourwebsite.com/og-image.png" /> -->

	<!-- Twitter -->
	<meta name="twitter:card" content="summary_large_image" />
	<!-- <meta name="twitter:url" content="https://yourwebsite.com/" /> -->
	<meta name="twitter:title" content="Year Progress Visualizer" />
	<meta
		name="twitter:description"
		content="Visualize the days left in the year with an interactive dot grid calendar."
	/>
	<!-- <meta name="twitter:image" content="https://yourwebsite.com/og-image.png" /> -->

	<!-- Additional SEO tags -->
	<meta name="robots" content="index, follow" />
	<meta name="author" content="Your Name" />
	<!-- <link rel="canonical" href="https://yourwebsite.com/" /> -->
</svelte:head>

{@render children()}
