<script>
	import { onMount } from 'svelte';
	import AbiDecoder from 'abi-decoder';
	// input value
	export let callData;
	export let to;

	let callDataString = '';

	onMount(async () => {
		console.log(`On Mount`);
		const res = await fetch(`/static/0x7a250d5630b4cf539739df2c5dacb4c659f2488d.json`);
		console.log(res);
		const abi = await res.json();
		if (callData && abi) {
			console.log(`abi: ${abi}`);
			AbiDecoder.addABI(abi);
			let decodedDataObject = AbiDecoder.decodeMethod(callData);
			callDataString = JSON.stringify(decodedDataObject);
		}
	});
</script>

<span>{callDataString}</span>
