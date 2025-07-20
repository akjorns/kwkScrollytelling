<script>
  import { onMount } from 'svelte';

  let scrollY = 0;
  let sceneTop = 0;
  let zoomProgress = 0;
  let sticky = false;
  let containerRef;

  let people = Array.from({ length: 100 }, (_, i) => {
    if (i === 0) return { name: 'Jenny', icon: '(•_•)' };
    if (i === 1) return { name: 'Grace', icon: '(^_^)' };
    return { name: '', icon: '' };
  });

  // Timing adjustments
  const startZoom = 200;  // when zoom starts
  const zoomDuration = 400; // shorter zoom duration
  const stickyStart = startZoom + zoomDuration; // when sticky should begin
  const stickyDuration = 600; // how long sticky lasts

  const onScroll = () => {
    scrollY = window.scrollY;

    if (containerRef) {
      const rect = containerRef.getBoundingClientRect();
      sceneTop = rect.top + scrollY;
    }

    const relativeScroll = scrollY - (sceneTop + startZoom);
    zoomProgress = Math.min(1, Math.max(0, relativeScroll / zoomDuration));
    
    // Sticky activates immediately after zoom completes
    sticky = relativeScroll >= zoomDuration && relativeScroll < zoomDuration + stickyDuration;
  };

  onMount(() => {
    if (containerRef) {
      const rect = containerRef.getBoundingClientRect();
      sceneTop = rect.top + window.scrollY;
    }

    window.addEventListener('scroll', onScroll);
    onScroll();
    return () => window.removeEventListener('scroll', onScroll);
  });
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

  .zoom-wrap {
    width: 80vw;
    max-width: 800px;
    margin: 0 auto;
    transition: transform 0.2s ease-out;
    transform-origin: center;
    will-change: transform;
  }

  .sticky {
    position: fixed;
    top: 150px;
    left: 50%;
    transform: translateX(-50%) scale(1) !important;
    z-index: 20;
    width: 800px;
  }

  .sticky-placeholder {
    height: 880px;
    width: 100%;
    visibility: hidden;
  }

  .grid {
    display: grid;
    grid-template-columns: repeat(10, 1fr);
    grid-gap: 10px;
    width: 100%;
  }

  .person-box {
    background: #DAA6D3;
    border-radius: 4px;
    height: 0;
    padding-bottom: 100%;
    position: relative;
    text-align: center;
    font-size: 0.8rem;
    color: black;
    overflow: hidden;
    transition: background-color 0.3s ease;
  }

  .sticky .person-box.left-group {
    background: #8bc34a;
  }

  .person-box span {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
  }

  .highlight {
    outline: 2px solid white;
  }
</style>

<div class="scene" bind:this={containerRef}>
  <div class="intro">
    <div>These are Jenny and Grace.</div>
    <div style="margin-top: 1rem;">But Jenny and Grace are just two examples...</div>
  </div>

  {#if sticky}
    <div class="sticky-placeholder"></div>
  {/if}

  <div
    class="zoom-wrap {sticky ? 'sticky' : ''}"
    style="transform: {sticky ? 'translateX(-50%) scale(1)' : `scale(${0.7 + zoomProgress * 0.3})`}"
  >
    <div class="grid">
      {#each people as person, i}
        <div
          class="person-box
                 {i === 0 || i === 1 ? 'highlight' : ''}
                 {sticky && (i % 10) < 5 ? 'left-group' : ''}"
        >
          <span>
            {#if i === 0}
              Jenny<br />{person.icon}
            {:else if i === 1}
              Grace<br />{person.icon}
            {/if}
          </span>
        </div>
      {/each}
    </div>
  </div>
</div>