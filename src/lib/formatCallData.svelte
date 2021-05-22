<script>
	import { onMount } from 'svelte';
	import AbiDecoder from 'abi-decoder';
	// input value
	export let callData;
	export let to;

	let decodedDataObject;
	let functionName;
	let functionParams = [];

	onMount(async () => {
		console.log(`On Mount`);
		const res = await fetch(`/static/0x7a250d5630b4cf539739df2c5dacb4c659f2488d.json`);
		console.log(res);
		const abi = await res.json();
		if (callData && abi) {
			//console.log(`abi: ${abi}`);
			AbiDecoder.addABI(abi);
			decodedDataObject = AbiDecoder.decodeMethod(callData);
			functionName = decodedDataObject.name;
			functionParams = decodedDataObject.params;
		}
	});
</script>

<div>
	<span class="signature">
		<span class="function">{functionName}</span> ({#each functionParams as p, i}
			<span class="datatype">{p.type}</span>
			{p.name}{#if i != functionParams.length - 1},&nbsp;{:else}){/if}
		{/each}
	</span>
	<div class="callvalues">
		<table>
			<thead>
				<tr> <th>param</th> <th>unit</th> <th>value</th></tr>
			</thead>
			{#each functionParams as p, i}
				<tr>
					<td>{p.name}</td>
					<td>{p.type}</td>
					<td>{p.value}</td>
				</tr>
			{/each}
		</table>
	</div>
	<!-- raw:<pre>{JSON.stringify(decodedDataObject, null, 2)} ) </pre> -->
</div>

<style>
	.signature {
		font-family: 'Courier New', Courier, monospace;
		font-size: 90%;
	}

	.function {
		font-weight: bold;
	}

	.datatype {
		color: var(--primary-color);
	}
	.callvalues {
		background-color: var(--tertiary-color);
		font-size: 80%;
	}

	td {
		padding-top: 0.2rem;
		padding-bottom: 0.2rem;
		word-wrap: anywhere;
	}
</style>
