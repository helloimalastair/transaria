<script lang="ts">
	/**
	 * Meta Tags for Social Embeds
	 */
	export let title: string;
	export let path: string;
	let url = `https://transaria.goalastair.com${path}`;
</script>
<svelte:head>
	<title>{ title }</title>
	<meta name="title" content={ title } />
	<meta name="theme-color" content="#0099CC" />
	<meta name="description" content="Monitor departures in real time on BART platforms. Works best in Landscape mode.  This project is not affiliated with BART, or any other transit provider." />
	<meta property="og:type" content="website" />
	<meta property="og:url" content={ url } />
	<meta property="og:title" content={ title } />
	<meta property="og:description" content="Monitor departures in real time on BART platforms. Works best in Landscape mode.  This project is not affiliated with BART, or any other transit provider." />
	<meta property="og:image" content="https://transaria.goalastair.com/icon.png" />
	<meta property="twitter:card" content="summary" />
	<meta property="twitter:url" content={ url } />
	<meta property="twitter:title" content={ title } />
	<meta property="twitter:description" content="Monitor departures in real time on BART platforms. Works best in Landscape mode.  This project is not affiliated with BART, or any other transit provider." />
	<meta property="twitter:image" content="https://transaria.goalastair.com/icon.png" />
</svelte:head>