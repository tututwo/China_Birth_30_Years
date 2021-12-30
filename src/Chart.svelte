<script>
  import { LayerCake, Svg, Html, Canvas } from "layercake"
  import { feature } from "topojson-client";
  import { geoMercator } from "d3-geo"
  import { scaleThreshold } from 'd3-scale';

  import Map from "./components/Map.svelte"
  import MapOutline from "./components/CN_MapOutline.svelte";

  import city from "./data/city2019_20.json"
  import data from "./data/data.csv"


  // Data
  let cityJSON = feature(city, city.objects.city2019_20)

  const flatData =  cityJSON.features.map(d => d.properties)

  let birthDomain = [0.2500,50000,100000,200000,340000]
  let birthRange = [
  'white',
  '#fffbd9', 
  '#cfedd5', 
  '#9ededf', 
  '#4fc6ef', 
  '#00aeff'
  ];
  $: birthScale = scaleThreshold().domain(birthDomain).range(birthRange)

  // let GRPDomain = []
  // let GRPRange = []
  // $: GRPScale = scaleThreshold().domain(GRPDomain).range(GRPRange)

  let hideTooltip = true;
  let evt

// $: console.log(countyData)
</script>

<!-- use bind, and event forwarding     on:click = {handleClick}-->

<figure>
  <!--! CN outline -->
  <LayerCake
    data = {cityJSON}
    position = "absolute"
    {flatData}
  >
    <Canvas>
      <MapOutline
        projection = {geoMercator}
      />
    </Canvas>

    <Canvas>
      <Map
        projection = {geoMercator}
        {birthScale}
        dataset = {data}
      ></Map>
    </Canvas>
  </LayerCake>



</figure>



<style>
  figure {
      display: block;
      position: relative;
      width: 100%;
      height: 80vh;
      min-height: 420px;
      margin: 0;
      user-select: none;
  }

</style>