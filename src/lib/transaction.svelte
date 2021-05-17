<script>
	import { utils, BigNumber } from 'ethers';
	import { Card, Details, Button } from 'svelte-chota';

	export let trx;
	// console.log(trx.gasPrice);
</script>

<div>
	<div class="card detail">
		{#if trx}
			<table>
				<thead>
					<tr>
						<th>Transaction </th>
						<th><a href="/tx/{trx.hash}">{trx.hash}</a></th>
					</tr>
				</thead>
				<tr>
					<td>
						<img src="https://icongr.am/entypo/address.svg?size=16&color=ff7f00" />
						Type
					</td>
					<td>
						{trx.type}
					</td>
				</tr>
				<tr>
					<td>
						<img src="https://icongr.am/entypo/book.svg?size=16&color=ff7f00" />
						Block
					</td>
					<td>
						<a href="/block/{trx.blockNumber}"> {trx.blockNumber}</a>
					</td>
				</tr>
				<tr
					><td><img src="https://icongr.am/entypo/clock.svg?size=16&color=ff7f00" /> From</td><td
						>{trx.from}
					</td></tr
				>
				<tr
					><td>
						<img src="https://icongr.am/entypo/drink.svg?size=16&color=ff7f00" />
						To</td
					><td>{trx.to} </td></tr
				>

				<tr
					><td>
						<img src="https://icongr.am/entypo/air.svg?size=16&color=ff7f00" />
						Value</td
					>
					<td>{trx.value} </td>
				</tr>
				<tr>
					<td>
						<img src="https://icongr.am/entypo/gauge.svg?size=16&color=ff7f00" />
						Gas Price</td
					>
					<td>
						{utils.formatUnits(trx.gasPrice, 'gwei')} Gwei
					</td>
				</tr>
				<tr>
					<td>
						<img src="https://icongr.am/entypo/gauge.svg?size=16&color=ff7f00" />
						Gas Limit</td
					>
					<td>
						{trx.gasLimit}
					</td>
				</tr>
				<tr>
					<td>
						<img src="https://icongr.am/entypo/bar-graph.svg?size=16&color=ff7f00" /> Data</td
					><td>{trx.data} </td></tr
				>
			</table>

			<Details>
				<span slot="summary">More detail... </span>
				<pre>{JSON.stringify(trx, null, 2)}</pre>
			</Details>
		{/if}

		{#if !trx}
			no transaction selected
		{/if}
	</div>
</div>

<style>
	td {
		padding-top: 0.2rem;
		padding-bottom: 0.2rem;
		word-wrap: anywhere;
	}

	button {
		padding: 0.2rem;
		background-color: pink;
	}
	.card.detail {
		border-style: solid;
		border-width: 1px;
		border-color: grey;
	}
</style>
