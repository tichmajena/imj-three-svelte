<script context="module">
  export const prerender = true;
  import Threetest from "$lib/components/Threetest.svelte";
</script>

<script>
  import Counter from "$lib/Counter.svelte";
</script>

<svelte:head>
  <title>Home</title>
</svelte:head>

<section class="relative h-screen w-full">
  <Threetest />
</section>
bxcv
