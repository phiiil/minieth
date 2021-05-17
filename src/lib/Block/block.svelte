<script>
	import { BigNumber } from 'ethers';
	import Hash from '$lib/hash.svelte';
	import { Card, Details, Button } from 'svelte-chota';

	export let block;
	if (block) {
		let percentGasUsed = ((block.gasUsed * 100) / block.gasLimit).toFixed(1);
	}
</script>

<div class="block">
	<div class="card blockdetail">
		{#if block}
			<table>
				<tr>
					<td>
						<img src="https://icongr.am/entypo/address.svg?size=16&color=ff7f00" />
						Block height</td
					>
					<td>
						<!-- <button seconday>&lt;</button> -->
						{block.number}
						<!-- <button>&gt;</button> -->
					</td>
				</tr>
				<tr>
					<td><img src="https://icongr.am/entypo/bookmarks.svg?size=16&color=ff7f00" /> Hash</td>
					<td>
						{block.hash}
					</td>
				</tr>
				<tr
					><td><img src="https://icongr.am/entypo/clock.svg?size=16&color=ff7f00" /> Timestamp</td
					><td>{new Date(block.timestamp * 1000)} </td></tr
				>
				<tr
					><td>
						<img src="https://icongr.am/entypo/drink.svg?size=16&color=ff7f00" />
						Miner</td
					><td>{block.miner} </td></tr
				>

				<tr
					><td>
						<img src="https://icongr.am/entypo/air.svg?size=16&color=ff7f00" />
						Transactions</td
					><td>{block.transactions.length} transactions</td></tr
				>
				<tr>
					<td> <img src="https://icongr.am/entypo/gauge.svg?size=16&color=ff7f00" /> Gas</td>
					<td>
						{((block.gasUsed * 100) / block.gasLimit).toFixed(1)}% gas used &nbsp;&nbsp;&nbsp; {block.gasUsed}
						/ {block.gasLimit}
					</td>
				</tr>
				<tr>
					<td>
						<img src="https://icongr.am/entypo/bar-graph.svg?size=16&color=ff7f00" /> Difficulty</td
					><td>{block.difficulty} </td></tr
				>
				<tr
					><td>
						<img src="https://icongr.am/entypo/code.svg?size=16&color=ff7f00" />
						Extra Data</td
					><td>{block.extraData} </td></tr
				>
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
</div>

<style>
	td {
		padding-top: 0.2rem;
		padding-bottom: 0.2rem;
	}

	button {
		padding: 0.2rem;
		background-color: pink;
	}
	.card.blockdetail {
		border-style: solid;
		border-width: 1px;
		border-color: grey;
	}
</style>
