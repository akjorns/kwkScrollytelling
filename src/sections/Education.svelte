<script>
  import { onMount } from 'svelte';

  let scrollY = 0;              // Current scroll position
  let sticky = false;           // Whether characters are fixed on screen
  let jennyActive = false;      // Jenny bubble visibility
  let graceActive = false;      // Grace bubble visibility

  let containerRef;             // Reference to outer container
  let sceneTop = 0;             // Top scroll position of this scene
  let naturalTop = 0;           // Original top offset for sticking

  const onScroll = () => {
    scrollY = window.scrollY;

    if (containerRef) {
      const rect = containerRef.getBoundingClientRect();
      sceneTop = rect.top + scrollY;
    }

    // Activate sticky position based on scene's top
    sticky = scrollY >= sceneTop && scrollY < sceneTop + 500;

    // Show Jenny's dialog between +100 and +300
    jennyActive = sticky && scrollY >= sceneTop + 100 && scrollY < sceneTop + 300;

    // Show Grace's dialog between +300 and +500
    graceActive = sticky && scrollY >= sceneTop + 300 && scrollY < sceneTop + 500;
  };

  onMount(() => {
    if (containerRef) {
      const rect = containerRef.getBoundingClientRect();
      sceneTop = rect.top + window.scrollY;
      naturalTop = sceneTop;
    }

    window.addEventListener('scroll', onScroll);
    onScroll();
    return () => window.removeEventListener('scroll', onScroll);
  });
</script>

<style>
  .scene {
    min-height: 90vh;
    display: flex;
    justify-content: center;
    align-items: start;
    padding-top: 150px;
    background: #034c36;
    color: white;
    font-family: monospace;
  }

  .characters-container {
    position: relative;
  }

  .characters {
    display: flex;
    gap: 4rem;
    position: relative;
  }

  .character-box {
    background: #DAA6D3;
    padding: 1rem;
    border-radius: 4px;
    text-align: center;
    position: relative;
    width: 150px;
    transition: all 0.3s ease;
  }

  .sticky-placeholder {
    height: 200px;
    width: 100%;
    visibility: hidden;
  }

  .sticky .characters {
    position: fixed;
    left: 50%;
    transform: translateX(-50%);
    z-index: 10;
  }

  .active {
    outline: 3px solid white;
  }

  .dialog {
    background: #FAD9F9;
    color: black;
    padding: 0.5rem 1rem;
    border-radius: 4px;
    position: absolute;
    top: 110%;
    left: 50%;
    transform: translateX(-50%);
    white-space: nowrap;
    box-shadow: 0 2px 6px rgba(0,0,0,0.2);
  }

  .character-box pre {
    font-size: 0.8rem;
    line-height: 1.2;
  }
</style>

<div class="scene">
  <div class="characters-container {sticky ? 'sticky' : ''}" bind:this={containerRef}>
    {#if sticky}
      <div class="sticky-placeholder"></div>
    {/if}

    <div
      class="characters"
      style={sticky ? `top: ${Math.max(150, naturalTop - scrollY)}px` : ''}
    >
      <div class="character-box {jennyActive ? 'active' : ''}">
        <pre>
  (\_/)
 ( •_•)
 />🍪
        </pre>
        <div><strong>Jenny</strong></div>
        {#if jennyActive}
          <div class="dialog">Jenny goes to public school</div>
        {/if}
      </div>

      <div class="character-box {graceActive ? 'active' : ''}">
        <pre>
  (^_^)
 (   )
 /___\
        </pre>
        <div><strong>Grace</strong></div>
        {#if graceActive}
          <div class="dialog">Grace also goes to public school</div>
        {/if}
      </div>
    </div>
  </div>
</div>
