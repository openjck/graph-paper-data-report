<script>
  import Metric from "./Metric.svelte";

  export let entrypoint;

  const metadataFetch = fetch(entrypoint)
    .then(response => response.json());

  function entrypointToMetricEndpoint(category, metric) {
    return entrypoint.replace('index.json', `${category}/${metric}/index.json`);
  }
</script>

{#await metadataFetch}
  <div>Awaiting</div>
{:then metadata}
  <h2>{metadata.title}</h2>
  {#if Array.isArray(metadata.description)}
    {#each metadata.description as paragraph}
      <p>{paragraph}</p>
    {/each}
  {:else}
    <p>{metadata.description}</p>
  {/if}
  {#each metadata.metrics as metricName}
    {#if metricName === 'hasFlash'}
      <Metric
        category={metadata.categories[0]}
        endpoint={entrypointToMetricEndpoint(metadata.categories[0], metricName)}
      />
    {/if}
  {/each}
{:catch err}
  <div>Error: {err.message}</div>
{/await}
