<script context="module">
	export const prerender = true;
</script>

<script>
	import { Row, Col, Button } from 'svelte-chota';
	import Block from '$lib/Block/block.svelte';
	import Blockrow from '$lib/blockrow.svelte';

	import { onMount, onDestroy } from 'svelte';
	import { ethers } from 'ethers';

	const provider = new ethers.providers.JsonRpcProvider({
		url: 'https://mainnet.infura.io/v3/9b9f057582ab4e2bbb44024ba59e56f7'
	});

	let pageSize = 6;
	let blockNumber;
	let displayedBlockNumber;
	let blocks = [];
	let selectedBlock = null;
	let moreCount = 0;

	// provider.on('block', (bn) => {
	// 	console.log(`[event] block: ${bn}`);
	// 	blockNumber = bn;
	// 	blockNumberChanged();
	// });

	function blockNumberChanged() {
		let ns = blocks.map((b) => b.number);
		displayedBlockNumber = Math.max(...ns);
		console.log(`displayBlockNumber: ${displayedBlockNumber}`);
		moreCount = blockNumber - displayedBlockNumber;
	}

	function compareByNumber(a, b) {
		return a.number - b.number;
	}

	function getBlocks() {
		console.log(`Updating ${pageSize} blocks starting at ${blockNumber}...`);
		for (let index = 0; index < pageSize; index++) {
			let bn = blockNumber - index;
			//console.log(`getting block #${bn}`);
			let b = provider.getBlock(bn).then((b) => {
				//console.log(`got block: ${bn}: ${JSON.stringify(b)}`);
				if (!blocks.map((b) => b.number).includes(b.number)) {
					blocks.push(b);
					blocks = blocks.sort(compareByNumber).slice(-6);
					// select first block returned
					if (!selectedBlock) {
						selectedBlock = b;
					}
					blockNumberChanged();
				}
			});
		}
	}

	function refreshToLatest() {
		provider.getBlockNumber().then((bn) => {
			console.log(`latest block number: ${bn}`);
			blockNumber = bn;
			getBlocks();
		});
	}

	onMount(() => {
		console.log(`on mount`);
		refreshToLatest();
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
	<div>
		<Button class="pull-left"><small>&lt; previous blocks</small></Button>
		<Button class="pull-right" on:click={refreshToLatest}>
			<small>{moreCount} more blocks &gt;</small>
		</Button>
	</div>

	<Blockrow {blocks} bind:selectedBlock />
	<div>
		<Row>
			<Col size="8">
				<Block block={selectedBlock} />
			</Col>
			<Col size="4">
				<Block block={selectedBlock} />
			</Col>
		</Row>
	</div>
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
</style>
