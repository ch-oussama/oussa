<script lang="ts">
  import { onMount } from 'svelte';

  // ---------------- WordsPullUp ----------------
  export let text: string = 'Prisma';
  export let showAsterisk: boolean = false;

  let containerRef: HTMLDivElement;
  let isInView = false;

  $: words = text.split(' ');

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

<div bind:this={containerRef} class="inline-flex flex-wrap">
  {#each words as word, i}
    {@const isLast = i === words.length - 1}
    <span
      class="inline-block relative words-pull-up-word"
      class:words-visible={isInView}
      style="margin-right: {isLast ? 0 : '0.25em'}; transition-delay: {i * 80}ms;"
    >
      {word}
      {#if showAsterisk && isLast}
        <span class="absolute top-[0.65em] -right-[0.3em] text-[0.31em]">*</span>
      {/if}
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
