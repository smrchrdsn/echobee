<script lang="ts">
	import favicon from '$lib/assets/favicon.svg';
	import '@radix-ui/colors/red.css';
	import '@radix-ui/colors/yellow.css';
	import '@radix-ui/colors/red-dark.css';
	import '@radix-ui/colors/yellow-dark.css';

	let { children } = $props();

	try {
		function toggleDarkMode(event: MediaQueryList | MediaQueryListEvent) {
			document.body.classList.toggle('dark', event.matches);
		}
		const mediaQuery = window.matchMedia('(prefers-color-scheme: dark)');
		toggleDarkMode(mediaQuery);
		mediaQuery.addEventListener('change', toggleDarkMode);
	} catch (error) {
		console.error('Error setting up dark mode:', error);
	}
</script>

<svelte:head>
	<link rel="icon" href={favicon} />
</svelte:head>

{@render children()}

<style lang="scss">
	:global {
		*,
		*::before,
		*::after {
			box-sizing: border-box;
		}

		html {
			block-size: 100%;
			inline-size: 100%;
		}

		body {
			display: flex;
			flex-direction: column;
			align-items: center;
			justify-content: center;
			block-size: 100%;
			inline-size: 100%;
			padding: 32px;
			background-color: var(--yellow-2);
			color: var(--yellow-11);
			font-family: 'Momo Trust Display', sans-serif;
			font-weight: 400;
			font-style: normal;
		}
	}
</style>
