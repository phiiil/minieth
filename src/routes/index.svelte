<script context="module">
	export const prerender = true;
</script>

<script>
	import Block from '$lib/Block/block.svelte';
	import Blockrow from '$lib/blockrow.svelte';

	import { onMount, onDestroy } from 'svelte';
	import { ethers } from 'ethers';

	const provider = new ethers.providers.JsonRpcProvider({
		url: 'https://mainnet.infura.io/v3/9b9f057582ab4e2bbb44024ba59e56f7'
	});

	let pageSize = 12;
	let blockNumber;
	const blocks = [];
	let selectedBlock = null;

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
				//blocks[bn] = b;
				blocks.push(b);
				blocks = blocks;
			});
		}
	}

	function selectBlock(b) {
		console.log(`block selected in main ${JSON.stringify(b)}`);
		selectedBlock = b;
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

<svelte:head>
	<title>Home</title>
</svelte:head>

<section>
	<h1>mini<strong>eth</strong></h1>
	<Blockrow {blocks} {selectBlock} />
	<Block block={selectedBlock} />
</section>

<style>
	section {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		flex: 1;
	}

	h1 {
		width: 100%;
	}

	.welcome {
		position: relative;
		width: 100%;
		height: 0;
		padding: 0 0 calc(100% * 495 / 2048) 0;
	}

	.welcome img {
		position: absolute;
		width: 100%;
		height: 100%;
		top: 0;
		display: block;
	}
</style>
