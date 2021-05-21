<script>
	import { utils } from 'ethers';

	// input value
	export let value;
	// units to convert to
	// https://docs.ethers.io/v5/api/utils/display-logic/#utils-formatUnits
	export let units = 'ether';
	export let unitsLabel = true;

	console.log(`format value ${value} into ${units}`);
	let valueString = '';
	if (value) {
		valueString = utils.formatUnits(value, units);
		if (unitsLabel) {
			valueString += ' ' + units;
		}
	}
</script>

<span>{valueString}</span>
