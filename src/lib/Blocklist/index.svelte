<script>
	console.log('Blocklist');
	import { onMount, onDestroy } from 'svelte';
	import { ethers } from 'ethers';

	const provider = new ethers.providers.JsonRpcProvider({
		url: 'https://mainnet.infura.io/v3/9b9f057582ab4e2bbb44024ba59e56f7'
	});

	let pageSize = 10;
	let blockNumber;
	const blocks = {};
	const blocksPage = [];

	// provider.on('block', (bn) => {
	// 	console.log(`[event] block: ${bn}`);
	// 	blockNumber = bn;
	// });

	function getBlocks() {
		console.log(`Updating ${pageSize} blocks starting at ${blockNumber}...`);
		for (let index = 0; index < pageSize; index++) {
			let bn = blockNumber - index;
			//console.log(`getting block #${bn}`);
			let b = provider.getBlock(bn).then((b) => {
				//console.log(`got block: ${bn}: ${JSON.stringify(b)}`);
				blocks[bn] = b;
				blocksPage.push(b);
				blocksPage = blocksPage;
			});
		}
	}

	onMount(() => {
		console.log(`on mount`);
		provider.getBlockNumber().then((bn) => {
			console.log(`latest block number: ${bn}`);
			blockNumber = bn;
			getBlocks();
		});
	});

	onDestroy(() => {
		console.log(`Removing event listeners`);
		provider.off('block');
	});
</script>

<div class="">
	<div>block number</div>
	<div class="counter-viewport">
		<div class="counter-digits">
			<strong>{blockNumber}</strong>
		</div>
	</div>
	<div class="blocklist">
		<p>list {blocksPage.length}</p>
		<ul>
			{#each blocksPage as b}
				<li>
					{b.number}
					<a target="_blank" href="">
						{b.transactions.length}
						{b.gasLimit}
						{b.gasUsed}
						{((b.gasUsed * 100) / b.gasLimit).toFixed(1)} %
					</a>
				</li>
			{/each}
		</ul>
	</div>
</div>

<style>
	.blocklist {
	}
	.counter {
		display: flex;
		border-top: 1px solid rgba(0, 0, 0, 0.1);
		border-bottom: 1px solid rgba(0, 0, 0, 0.1);
		margin: 1rem 0;
	}
	.counter-viewport {
		width: 28em;
		height: 4em;

		text-align: center;
		position: relative;
	}

	.counter-viewport strong {
		position: absolute;
		display: block;
		width: 100%;
		height: 100%;
		font-weight: 400;
		color: var(--accent-color);
		font-size: 4rem;
		display: flex;
		align-items: center;
		justify-content: center;
	}

	.counter-digits {
		position: absolute;
		width: 100%;
		height: 100%;
	}
</style>
