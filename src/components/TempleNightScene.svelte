<script lang="ts">
  import { onMount } from 'svelte';
  import { createTempleNightRenderer } from '../shaders/temple-night/templeNightRenderer.js';
  import '../shaders/threeui.css';

  export let variant = 'temple-night';
  export let className = '';
  
  let canvasRef: HTMLCanvasElement;
  let hostRef: HTMLDivElement;
  let state: 'loading' | 'ready' | 'unavailable' = 'loading';
  let errorMessage = '';

  onMount(() => {
    if (!hostRef || !canvasRef) return;

    let renderer: any;
    try {
      renderer = createTempleNightRenderer(canvasRef);
    } catch (error) {
      errorMessage = error instanceof Error ? error.message : "Unknown renderer error";
      state = 'unavailable';
      return;
    }
    if (!renderer) {
      state = 'unavailable';
      return;
    }

    let frame = 0;
    let visible = true;
    let disposed = false;
    let rendered = false;

    const schedule = () => {
      if (!disposed && visible && !document.hidden && !frame) {
        frame = requestAnimationFrame(render);
      }
    };

    const render = (time: number) => {
      frame = 0;
      renderer.render(time);
      if (!rendered) {
        rendered = true;
        state = 'ready';
      }
      if (!renderer.reducedMotion) schedule();
    };

    const resize = () => {
      renderer.resize();
      schedule();
    };

    const setPointer = (event: PointerEvent) => {
      const bounds = canvasRef.getBoundingClientRect();
      const x = ((event.clientX - bounds.left) / Math.max(1, bounds.width)) * 2 - 1;
      const y = 1 - ((event.clientY - bounds.top) / Math.max(1, bounds.height)) * 2;
      renderer.setPointer(x, y, true);
      schedule();
    };

    const clearPointer = () => {
      renderer.setPointer(0, 0, false);
      schedule();
    };

    const onVisibility = () => {
      if (document.hidden && frame) {
        cancelAnimationFrame(frame);
        frame = 0;
      } else {
        schedule();
      }
    };

    const resizeObserver = new ResizeObserver(resize);
    resizeObserver.observe(hostRef);
    
    const intersectionObserver = new IntersectionObserver(([entry]) => {
      visible = entry?.isIntersecting ?? true;
      if (!visible && frame) {
        cancelAnimationFrame(frame);
        frame = 0;
      } else {
        schedule();
      }
    });
    intersectionObserver.observe(hostRef);
    
    canvasRef.addEventListener("pointermove", setPointer, { passive: true });
    canvasRef.addEventListener("pointerenter", setPointer, { passive: true });
    canvasRef.addEventListener("pointerleave", clearPointer, { passive: true });
    window.addEventListener("blur", clearPointer);
    document.addEventListener("visibilitychange", onVisibility);
    resize();

    return () => {
      disposed = true;
      if (frame) cancelAnimationFrame(frame);
      resizeObserver.disconnect();
      intersectionObserver.disconnect();
      canvasRef.removeEventListener("pointermove", setPointer);
      canvasRef.removeEventListener("pointerenter", setPointer);
      canvasRef.removeEventListener("pointerleave", clearPointer);
      window.removeEventListener("blur", clearPointer);
      document.removeEventListener("visibilitychange", onVisibility);
      renderer.dispose();
    };
  });
</script>

<div class={`temple-night-scene ${className}`} bind:this={hostRef} data-state={state}>
  <canvas
    bind:this={canvasRef}
    class={`temple-night-canvas ${state === 'ready' ? 'is-ready' : ''}`}
    aria-label="Interactive Kage mountain temple world after dark"
    style="pointer-events: auto;"
  ></canvas>
  {#if state === 'unavailable'}
    <p class="temple-night-unavailable" role="status">WebGL is unavailable: {errorMessage || 'unsupported context'}.</p>
  {/if}
</div>

<style>
  .temple-night-scene {
    position: relative;
    width: 100%;
    height: 100%;
  }
</style>
