<script>
	import Chart from "./Chart.svelte"
</script>

<main>
	<section>
		<h4 class = "headline">/h4>
	</section>

	<Chart/>
</main>

<style>
	main {
		font-family: "Inter", sans-serif;
		padding: 0 1em;
		max-width: 50em;
		margin: 0 auto;
	}

	.headline {
		font-family: Gordita,Helvetica,Arial,sans-serif;
		font-size: 1.3rem;
		line-height: 1.2rem;
		font-weight: 600;
		color: #4a4a4a;
		margin-bottom: 10px;
	}

	h4 {
		text-align: center;
	}
</style>