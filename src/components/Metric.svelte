<script>
  import DataGraphic from '@graph-paper/datagraphic';
  import LeftAxis from '@graph-paper/guides/LeftAxis.svelte';
  import BottomAxis from '@graph-paper/guides/BottomAxis.svelte';
  import { Line } from '@graph-paper/elements';

  export let category;
  export let endpoint;

  const dataFetch = fetch(endpoint)
    .then(response => response.json())
    .then(data => {
      return data.data.populations[category].map(dataPoint => {
        const newDataPoint = { ...dataPoint };
        newDataPoint.x = new Date(dataPoint.x);
        return newDataPoint;
      });
    });
</script>

{#await dataFetch}
  <div>Awaiting</div>
{:then data}
  <DataGraphic
    xType="time"
    yType="linear"
    yMin={0}
    width={500}
    height={300}>
    <LeftAxis />
    <BottomAxis />
    <Line {data} />
  </DataGraphic>
{:catch err}
  <div>Error {err.message}</div>
{/await}
