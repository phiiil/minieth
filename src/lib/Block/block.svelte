<script>
	import { BigNumber } from 'ethers';
	import Hash from '$lib/hash.svelte';
	import { Details, Button } from 'svelte-chota';

	export let block;

	let percentGasUsed = block ? ((block.gasUsed * 100) / block.gasLimit).toFixed(1) : 0;
</script>

<div class="block">
	{#if block}
		<table>
			<tr
				><td>Block height</td><td
					><button seconday>&lt;</button> {block.number} <button>&gt;</button></td
				></tr
			>
			<tr
				><td>Hash/Parent/Nonce</td><td>
					<Details>
						<span slot="summary"
							>Hash:{block.hash.slice(2, 9)} &nbsp;&nbsp; Parent:{block.parentHash.slice(2, 9)}
						</span>

						<div>
							<pre>
Hash: {block.hash}
Parent Hash:{block.parentHash}
Nonce:{block.nonce}
                    </pre>
						</div>
					</Details>
				</td></tr
			>

			<tr><td>Timestamp</td><td>{new Date(block.timestamp * 1000)} </td></tr>
			<tr><td>Miner</td><td>{block.miner} </td></tr>

			<tr><td>Transactions</td><td>{block.transactions.length} transactions</td></tr>
			<tr>
				<td>Gas</td>
				<td>
					{percentGasUsed}% gas used &nbsp;&nbsp;&nbsp; {block.gasUsed} / {block.gasLimit}
				</td>
			</tr>
			<tr><td>Difficulty</td><td>{block.difficulty} </td></tr>
			<tr><td>Extra Data</td><td>{block.extraData} </td></tr>
		</table>

		<Details>
			<span slot="summary">More detail... </span>
			<pre>{JSON.stringify(block, null, 2)}</pre>
		</Details>
	{/if}

	{#if !block}
		no block selected
	{/if}
</div>

<style>
	@import 'chota';
</style>
