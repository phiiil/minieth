<script context="module">
	// Page preload
	// Get Block With Transactions
	import { ethers } from 'ethers';
	export async function load({ page, fetch, session, context }) {
		const provider = new ethers.providers.JsonRpcProvider({
			url: 'https://mainnet.infura.io/v3/9b9f057582ab4e2bbb44024ba59e56f7'
		});
		let bn = Number(page.params.number);
		//console.log(`getting block ${bn}`);
		return provider.getBlockWithTransactions(bn).then((b) => {
			//console.log(`got block: ${bn}: ${JSON.stringify(b)}`);
			return {
				props: {
					block: b
				}
			};
		});

		// return {
		// 	status: res.status,
		// 	error: new Error(`Could not load ${url}`)
		// };
	}
</script>

<script>
	import Block from '$lib/Block/block.svelte';
	import Transaction from '$lib/transaction.svelte';
	export let block;
</script>

<svelte:head>
	<title>Block</title>
</svelte:head>

<section>
	<h1>Block #{block.number}</h1>
	<Block {block} />
	{#each block.transactions.slice(0, 5) as trx}
		<Transaction {trx} />
	{/each}
</section>
