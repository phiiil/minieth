<script>
	import { BigNumber } from 'ethers';
	export let block;
	let percentGasUsed = ((block.gasUsed * 100) / block.gasLimit).toFixed(1);
</script>

<div class="block">
	<h1>This is a block {block.number}</h1>
	<table>
		<tr><td>Block height</td><td><button /> {block.number} <button /></td></tr>
		<tr><td>Timestamp</td><td>{new Date(block.timestamp * 1000)} </td></tr>
		<tr><td>Transactions</td><td>{block.transactions.length} transactions</td></tr>
		<tr>
			<td>Gas</td>
			<td>
				{percentGasUsed}% gas used &nbsp;&nbsp;&nbsp; {block.gasUsed} / {block.gasLimit}
			</td>
		</tr>
	</table>
	<pre>{JSON.stringify(block, null, 2)}</pre>
</div>
