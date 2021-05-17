<script>
	import { Row, Col, Card, Button } from 'svelte-chota';
	import { formatDistance } from 'date-fns';
	export let blocks;
	export let selectedBlock;

	function isSelected(b) {
		return selectedBlock ? selectedBlock.number == b.number : false;
	}
</script>

<div>
	<Row>
		{#each blocks as b}
			<Col size="2">
				<div
					class="card {isSelected(b) ? 'selected' : ''}"
					on:click={() => {
						console.log(`Block selected #` + b.number);
						selectedBlock = b;
						blocks = blocks; // force refresh
					}}
				>
					<!-- <h3 slot="header"></h3> -->
					<header>
						<h4>{b.number}</h4>
					</header>

					<small>
						{formatDistance(new Date(b.timestamp * 1000), new Date(), { addSuffix: true })}<br />
						{b.transactions.length} txs <br />
						{(b.gasUsed / b.transactions.length).toFixed(0)} Gas/Tx <br />
					</small>
					<div class="footer is-right">
						<a href="/block/{b.number}">
							<img
								src="https://icongr.am/entypo/chevron-with-circle-right.svg?size=16&color=ff7f00"
								alt="block detail"
							/>
						</a>
					</div>
				</div>
			</Col>
		{/each}
	</Row>
</div>

<style>
	.card {
		border-style: solid;
		border-width: 1px;
		border-color: grey;
	}
	.selected {
		border-style: solid;
		border-color: orange;
		background: rgb(254, 230, 187);
	}
</style>
