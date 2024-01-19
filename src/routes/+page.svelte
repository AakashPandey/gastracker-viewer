<svelte:head>
	<title>Home</title>
	<meta name="description" content="Gas tracker" />
</svelte:head>

<script>
	import '@carbon/charts-svelte/styles.css'
	import { LineChart } from '@carbon/charts-svelte'
  import { onMount } from 'svelte';

	let gasData = [];
	onMount(async function () {
		const response = await fetch('http://localhost:3000/gasprices');
		const data = await response.json();
		const d = [];
		data.forEach((e)=>{
			d.push({
				date: new Date(e['on']),
				value: parseFloat(e['cost'].toString().split('$')[1].replace(')','')),
				group: 'USD',
			})
		})
		console.log(gasData);
		gasData = [...d];
});

const options = {
  title: 'Med Gas Price (Snowtrace)',
  axes: {
    left: {
    },
    bottom: {
      scaleType: 'time'
    }
  },
  legend: {
    clickable: false
  },
  height: '400px'
}
</script>



<section class="mx-14 mt-8">
	<div class="w-[75%] m-auto mt-16">
		<LineChart data={gasData} options={options} />
	</div>

</section>
