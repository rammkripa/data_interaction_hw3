<script>
  import * as d3 from 'd3';
  import { legendColor } from 'd3-svg-legend';
	import {onMount} from 'svelte';
  import Map from './Map.svelte';
  import Histogram from './Histogram.svelte';
  import Scatter from './Scatter.svelte';
  import Bar from './Bar.svelte';
  import HorizontalBar from './HorizontalBar.svelte';

	export let data = [];
  export let fullData = [];

  let whichYear;
  let numBreaches;

	onMount(async function() {
    // load data from csv (source: https://chicagohealthatlas.org/download)
    let table = d3.csv('data_breaches.csv', (d) => ({
          ...d,
          'Entity': d['Entity'],
          'Year': +d['Year'],
          'NumRecords': +d['Records'],
          'OrgType': d['Organization type'],
          'Method': d['Method']
        }));
    data = await table;
    fullData = data;
    numBreaches = data.length;
    whichYear = d3.min(data, d => d['Year']);
	});

let scatterVar1 = 'Year';
let scatterVar2 = 'NumRecords';

let scatterSelection = [];

let filter1 = [];
let filter2 = [];
let filter3 = [];
let variable2 = 'OrgType';
let variable3 = 'Method';

function updateData() {
  if (filter1.length === 0 && filter2.length === 0 && filter3.length === 0) {
    data = fullData;
    return;
  }
  data = fullData.filter(d => {
    return filter1.includes(d['NumRecords']) && filter2.includes(d['OrgType']) && filter3.includes(d['Method']);
  });
}

</script>

<main>
  <h1>Data Breaches</h1>
  

  <div class="flex-container row">
    <div class="explainertext"> <p> Since the year {whichYear}, there have been {numBreaches} data breaches. </p> </div>
    <div class="flex-container col">
      <div class="hist"><Histogram data={data} fullData={fullData} variable={'NumRecords'} bind:filter={filter1} update={updateData}></Histogram></div>
      <div class="bar"><HorizontalBar data={data} fullData={fullData} variable={variable2} bind:filter={filter2} update={updateData}></HorizontalBar></div>
      <div class="bar"><HorizontalBar data={data} fullData={fullData} variable={variable3} bind:filter={filter3} update={updateData}></HorizontalBar></div>
      <div class="scatter"><Scatter data={data} fullData={fullData} variable1={scatterVar1} variable2={scatterVar2} bind:selectedPoints={scatterSelection} update={updateData}></Scatter></div>
    </div>
  </div>
</main>

<style>
  /* .flex-container {
    display: flex;
    justify-content: center;  
    height: 100%;
    padding: 15px;
    gap: 5px;
  }

  .flex-container > div{
    padding: 8px;
  }

  .flex-container .row {
    flex-direction: row;  
  }

  .flex-container .col {
    flex-direction: column;  
  }

  .map { 
    flex-grow:1;
  }
			
  .hist { 
    flex-grow:0;
  } */
</style>