<script lang="ts">
	import { MetaTags } from "$components";
	import { onMount } from "svelte";
	import type { Directions, StationsShort, StatDir } from "$lib";

	let selectedStation: StationsShort;
	let selectedDirection: Directions;
	let validDirections: Directions[] = ["N", "S"];
	let stationDirections: [string, StatDir][] | null = null;
	let mapping: Record<StationsShort, StatDir> | null = null;
	let route = "";
	$: route = `/display/${selectedStation}-${selectedDirection}`;
	$: {
		if(mapping && selectedStation) {
			validDirections = mapping[selectedStation].dir;
			if(!validDirections.includes(selectedDirection)) {
				selectedDirection = validDirections[0];
			}
		}
	}
	onMount(async () => {
		/**
		 * When the page has loaded client-side, download the station mapping from the server.
		 * This includes all stations, along with the directions they support.
		 * When a station has been selected, the directional dropdown is narrowed to only the supported
		 * direction(s).
		 */
		const statMapping = await (await fetch("/statdir")).json() as Record<StationsShort, StatDir>;
		const intermidiary = Object.entries(statMapping);
		selectedStation = intermidiary[0][0] as StationsShort;
		stationDirections = intermidiary;
		mapping = statMapping;
	});
</script>

<MetaTags title="TransAria" path="/" />

<div class="flex items-center justify-center w-screen h-screen bg-gray-900">
	<div class="w-2/5 h-4/5 bg-white rounded-3xl px-10 py-5">
		<h1 class="text-6xl text-center font-bold">TransAria</h1>
		<p class="text-center italic">Monitor departures in real time on BART platforms. Works best in Landscape mode. This project is not affiliated with BART, or any other transit provider.</p>
		<p class="text-center">Source Available <a href="https://github.com/helloimalastair/transaria" target="_blank" class="underline text-blue-line">on Github</a> under the GNU GPL v3 License.</p>
		<div class="flex flex-col items-center mt-7 gap-5">
			<label for="Station" class="text-4xl font-bold">Station</label>
			<select name="Station" class="text-2xl px-3 py-1 rounded-md" bind:value={selectedStation}>
				{#if stationDirections !== null}
					{#each stationDirections as station}
						<option value="{station[0]}">{station[1].long}</option>
					{/each}
				{/if}
			</select>
			<label for="Direction" class="text-4xl font-bold">Direction</label>
			<select name="Direction" class="text-2xl px-3 py-1 rounded-md" bind:value={selectedDirection}>
				{#each validDirections as direction}
					<option value="{direction}">{direction === "N" ? "Northbound" : "Southbound"}</option>
				{/each}
			</select>
			<a href={route} class="bg-blue-line text-white text-5xl px-5 py-2 rounded-2xl mt-4 font-bold">GO!</a>
		</div>
	</div>
</div>