<script>
	import { utils } from 'ethers';

	// input value
	export let value;
	// units to convert to
	// https://docs.ethers.io/v5/api/utils/display-logic/#utils-formatUnits
	export let units = 'ether';
	export let showUnits = true;

	console.log(`format value ${value} into ${units}`);
	let valueString = '';
	if (value) {
		valueString = utils.formatUnits(value, units);
	}
</script>

<span
	>{valueString}
	{#if showUnits} <span class="units">{units}</span> {/if}</span
>

<style>
	.units {
		color: var(--primary-color);
		text-transform: capitalize;
	}
</style>
