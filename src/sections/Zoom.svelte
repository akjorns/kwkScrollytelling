<script>
  import { onMount } from "svelte";

  let scrollY = 0;
  let sceneTop = 0;
  let containerRef;

  // Calculate progress from 0 to 1 as user scrolls past sceneTop
  let progress = 0;

  const onScroll = () => {
    scrollY = window.scrollY;

    if (containerRef) {
      const rect = containerRef.getBoundingClientRect();
      sceneTop = rect.top + scrollY;
    }

    // Progress from 0 to 1 over 500px scroll after sceneTop
    progress = Math.min(1, Math.max(0, (scrollY - sceneTop) / 500));
  };

  onMount(() => {
    if (containerRef) {
      const rect = containerRef.getBoundingClientRect();
      sceneTop = rect.top + window.scrollY;
    }

    window.addEventListener("scroll", onScroll);
    onScroll();
    return () => window.removeEventListener("scroll", onScroll);
  });

  const totalBoxes = 100;
  const halfBoxes = totalBoxes / 2;
  const boxes = Array(totalBoxes).fill(0);
</script>

<style>
  .scene {
    min-height: 350vh;
    padding: 150px 1rem 400px;
    background: #034c36;
    color: white;
    font-family: monospace;
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  .intro {
    text-align: center;
    margin-bottom: 4rem;
    font-size: 1.5rem;
  }

  .grid {
    display: grid;
    grid-template-columns: repeat(10, 1fr);
    grid-gap: 10px;
    width: 80vw;
    max-width: 800px;
  }

  .person-box {
    background: pink;
    border-radius: 4px;
    height: 0;
    padding-bottom: 100%;
    position: relative;
    transition: background-color 0.3s ease;
  }

  .person-box.purple {
    background: purple;
  }
</style>

<div class="scene" bind:this={containerRef}>
  <div class="intro">
    <div>These are Jenny and Grace.</div>
    <div style="margin-top: 1rem;">But Jenny and Grace are just two examples...</div>
  </div>

  <div class="grid">
    {#each boxes as _, i}
      <div class="person-box {i < halfBoxes * progress ? 'purple' : ''}"></div>
    {/each}
  </div>
</div>
