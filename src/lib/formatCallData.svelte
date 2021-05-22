<script>
	import { onMount } from 'svelte';
	import AbiDecoder from 'abi-decoder';
	// input value
	export let callData;
	export let to;

	let decodedDataObject;
	let functionName;
	let functionParams = [];
	let errorMessage;

	onMount(async () => {
		console.log(`On Mount`);
		try {
			let apiKey = import.meta.env.VITE_ETHERSCAN_APIKEY;
			const res = await fetch(
				`https://api.etherscan.io/api?module=contract&action=getabi&address=${to}&apikey=${apiKey}`
			);
			console.log(`res : ${res}`);
			const abiResponse = await res.json();
			if (abiResponse.status == 1) {
				if (callData) {
					const abi = JSON.parse(abiResponse.result);
					console.log(`decoding abi of type: ${typeof abi}`);
					AbiDecoder.addABI(abi);
					decodedDataObject = AbiDecoder.decodeMethod(callData);
					console.log(`decoded ${decodedDataObject}`);
					functionName = decodedDataObject.name;
					functionParams = decodedDataObject.params;
				}
			} else {
				// no abi available
				console.log(`abi: ${JSON.stringify(abiResponse)}`);
				errorMessage = abiResponse.result;
			}
		} catch (e) {
			console.log(e);
		}
	});
</script>

<div>
	{#if errorMessage}
		<span>{errorMessage}</span>
	{:else}
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
	{/if}
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
