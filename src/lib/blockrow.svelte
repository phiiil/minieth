<script>
	import { Row, Col, Card, Button } from 'svelte-chota';
	import { formatDistance } from 'date-fns';
	export let blocks;
	export let selectBlock;
</script>

<div>
	<Row>
		{#each blocks as b}
			<Col size="2">
				<Card
					on:click={() => {
						console.log(`Block selected #` + b.number);
						selectBlock(b);
					}}
				>
					<h3 slot="header">{b.number}</h3>

					<small
						>{formatDistance(new Date(b.timestamp * 1000), new Date())}<br />
						{b.transactions.length} txs <br />
						{(b.gasUsed / b.transactions.length).toFixed(0)} Gas/Tx <br />
					</small>
					<div slot="footer" class="is-right"><small>footer</small></div>
				</Card>
			</Col>
		{/each}
	</Row>
</div>

<style>
	.box {
		background-color: pink;
		border-width: 2px;
	}
</style>
