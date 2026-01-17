<script lang="ts">
	import favicon from '$lib/assets/favicon.svg';

	let { children } = $props();
</script>

<svelte:head>
	<link rel="icon" href={favicon} />
	<title>Portfolio - Full Stack Developer</title>
	<meta name="description" content="Portfolio of a full stack developer specializing in modern web technologies" />
</svelte:head>

{@render children()}
