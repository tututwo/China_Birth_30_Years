<script>
	import { getContext, createEventDispatcher } from 'svelte';
  import { scaleCanvas } from 'layercake';
	import { geoPath } from 'd3-geo';

  export let projection

	// const { data, width, height } = getContext('LayerCake');
	const { data, width, height } = getContext('LayerCake');
  const { ctx } = getContext('canvas');

	/* --------------------------------------------
	 * Add this optional export in case you want to plot only a subset of the features
	 * while keeping the zoom on the whole geojson feature set
	 */
	export let features = $data.features;
  // console.log(features)
	/* --------------------------------------------
	 * Here's how you would do cross-component hovers
	 */
	// const dispatch = createEventDispatcher();

  $: projectionFn = projection().fitSize([$width, $height], $data)
  $: geoPathFn = geoPath(projectionFn)

  $: {
    if ($ctx && geoPath) {
      scaleCanvas($ctx, $width, $height);
      $ctx.clearRect(0, 0, $width, $height);

      features.forEach(feature => {
        $ctx.beginPath();
        // Set the context here since setting it in `$: geoPath` is a circular reference
        geoPathFn.context($ctx);
        geoPathFn(feature);

        $ctx.fillStyle = "white";
        // $ctx.fillStyle = $zGet(feature.properties); // Use this if making a choropleth
        $ctx.fill();

        $ctx.lineWidth = 2;
        $ctx.strokeStyle = "black";
        $ctx.stroke();
      });
    }
  }
</script>

<!-- {#each features as feature}
  <path
    class = 'feature-path'
    fill = "none"
    d = {geoPathFn(feature)}

  />
{/each} -->

<style>
	/* .feature-path {
		stroke-width: 1;
    stroke: black;
	} */
</style> 
