<script>
	import { getContext } from 'svelte';
	import { geoPath } from 'd3-geo';
  import { scaleCanvas } from 'layercake';

  export let projection, birthScale, dataset

	const { data, width, height, z, zGet } = getContext('LayerCake');
  const { ctx } = getContext("canvas")

	export let features = $data.features;
	/* --------------------------------------------
	 * Here's how you would do cross-component hovers
	 */
	// const dispatch = createEventDispatcher();

  $: projectionFn = projection().fitSize([$width, $height], $data)
  $: geoPathFn = geoPath(projectionFn)

  $: featureProps = features.map(feature => {
      const {properties: {行政区划_c: City_EN}} = feature

      const GRP1 = dataset.filter(d => d["City"] == City_EN)
      const Birth_Pop1 = dataset.filter(d => d["City"] == City_EN)
      if (GRP1.length == 0 || Birth_Pop1.length == 0) {
        GRP1[0] = {GRP: 0, Birth_Pop: 0}
        console.log(1)
        Birth_Pop1[0] = {GRP: 0, Birth_Pop: 0}
      } else {
        GRP1
        Birth_Pop1
      }
      const GRP = +GRP1[0]["GRP"]
      const Birth_Pop = +Birth_Pop1[0]["Birth_Pop"]


      return {
        ...feature,
        Birth_Pop,
        GRP
      }
    })

  $: {
    if ($ctx && geoPath) {
      scaleCanvas($ctx, $width, $height) //! define canvas dimensions
      // $ctx.clearRect(0, 0, $width, $height)
      // $ctx.width = $width
      // $ctx.height = $height
      featureProps.forEach(feature => {
        $ctx.beginPath()

        geoPathFn.context($ctx)
        geoPathFn(feature)

        $ctx.fillStyle = `${birthScale(feature.Birth_Pop)}`

        // console.log($ctx.fillStyle)
        $ctx.fill()

        $ctx.linewidth = 0.1
        $ctx.strokeStyle = "#ddd";
        $ctx.stroke();
      })
    }
  }

  // $: featuresProps = features.map(feature => {
  //   const fill = $z(feature.properties)? colorScale($z(feature.properties)): "white"
  //   // console.log($zGet(feature.properties))
  //   return {
  //     ...feature,
  //     fill
  //   }
  // })
</script>

<!-- {#each featuresProps as feature}
  <path
    class = 'feature-path'
    fill = {feature.fill}
    d = {geoPathFn(feature)}

  />
{/each} -->

<style>
	.feature-path {
		stroke: none;
		stroke-width: 0;
	}
</style> 