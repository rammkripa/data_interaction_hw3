<script>
  import * as d3 from 'd3';
  import { legendColor } from 'd3-svg-legend';
	import {onMount} from 'svelte';
  import Map from './Map.svelte';
  import Histogram from './Histogram.svelte';
  import Scatter from './Scatter.svelte';

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

function updateData() {

}

</script>

<main>
  <h1>Data Breaches</h1>
  

  <div class="flex-container row">
    <div class="explainertext"> <p> Since the year {whichYear}, there have been {numBreaches} data breaches. </p> </div>
    <div class="flex-container col">
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