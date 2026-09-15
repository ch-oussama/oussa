<script lang="ts">
  import { onMount } from 'svelte';

  export let segments: { text: string; className?: string }[] = [];
  export let className: string = '';
  export let style: string = '';

  let containerRef: HTMLDivElement;
  let isInView = false;

  $: words = (() => {
    const out: { word: string; className?: string }[] = [];
    segments.forEach((seg) => {
      seg.text.split(' ').forEach((w) => {
        if (w) out.push({ word: w, className: seg.className });
      });
    });
    return out;
  })();

  onMount(() => {
    const observer = new IntersectionObserver(
      (entries) => {
        entries.forEach((entry) => {
          if (entry.isIntersecting) {
            isInView = true;
            observer.disconnect();
          }
        });
      },
      { threshold: 0.1 }
    );
    if (containerRef) observer.observe(containerRef);
    return () => observer.disconnect();
  });
</script>

<div bind:this={containerRef} class="inline-flex flex-wrap justify-center {className}" {style}>
  {#each words as w, i}
    <span
      class="inline-block words-pull-up-word {w.className ?? ''}"
      class:words-visible={isInView}
      style="margin-right: 0.25em; transition-delay: {i * 80}ms;"
    >
      {w.word}
    </span>
  {/each}
</div>

<style>
  .words-pull-up-word {
    opacity: 0;
    transform: translateY(20px);
    transition:
      opacity 0.6s cubic-bezier(0.16, 1, 0.3, 1),
      transform 0.6s cubic-bezier(0.16, 1, 0.3, 1);
    will-change: opacity, transform;
  }
  .words-visible {
    opacity: 1;
    transform: translateY(0);
  }
</style>
