<script context="module">
	// Page preload
	// Get Block With Transactions
	import { ethers, BigNumber } from 'ethers';

	export async function load({ page, fetch, session, context }) {
		const provider = new ethers.providers.JsonRpcProvider({
			url: 'https://mainnet.infura.io/v3/9b9f057582ab4e2bbb44024ba59e56f7'
		});
		let hash = page.params.hash;
		console.log(`getting tx ${hash}`);
		return provider.getTransaction(hash).then((tx) => {
			//console.log(`got tx: ${JSON.stringify(tx)}`);
			return {
				props: {
					tx: tx
				}
			};
		});
		return {
			status: res.status,
			error: new Error(`Could not load ${url}`)
		};

		return {
			props: {
				tx: {}
			}
		};
	}
</script>

<script>
	import Transaction from '$lib/transaction.svelte';
	export let tx;
</script>

<svelte:head>
	<title>Transaction</title>
</svelte:head>

<section>
	<Transaction trx={tx} />
</section>
