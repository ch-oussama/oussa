<script lang="ts">
  import { onMount } from 'svelte';
  import { createEventDispatcher } from 'svelte';
  import WordsPullUp from './WordsPullUp.svelte';

  const dispatch = createEventDispatcher();

  let mounted = false;
  onMount(() => {
    requestAnimationFrame(() => {
      mounted = true;
    });
  });
</script>

<section class="h-full w-full">
  <div class="relative h-full w-full overflow-hidden">

    <!-- Background video -->
    <video
      autoplay
      loop
      muted
      playsinline
      class="absolute inset-0 h-full w-full object-cover"
      src="/hero-video.mp4"
    ></video>

    <!-- Noise overlay -->
    <div class="noise-overlay pointer-events-none absolute inset-0 opacity-[0.7] mix-blend-overlay"></div>

    <!-- Gradient overlay -->
    <div class="pointer-events-none absolute inset-0 bg-gradient-to-b from-black/30 via-transparent to-black/60"></div>

    <!-- Hero content -->
    <div class="absolute bottom-0 left-0 right-0 px-4 pb-2 sm:px-6 md:px-10">
      <div class="grid grid-cols-12 items-end gap-4">

        <div class="col-span-12 lg:col-span-8">
          <h1
            class="font-medium leading-[0.85] tracking-[-0.07em] text-[26vw] sm:text-[24vw] md:text-[22vw] lg:text-[20vw] xl:text-[19vw] 2xl:text-[20vw]"
            style="color: #E1E0CC;"
          >
            <WordsPullUp text="Oussa" showAsterisk={true} />
          </h1>
        </div>

        <div class="col-span-12 flex flex-col gap-5 pb-6 lg:col-span-4 lg:pb-10">

          <p
            class="prisma-fade-up text-xs sm:text-sm md:text-base"
            class:prisma-visible={mounted}
            style="line-height: 1.2; color: rgba(225, 224, 204, 0.7); transition-delay: 500ms;"
          >
            Game developer and senior engineer building immersive experiences from Algeria. Crafting mechanics, systems, and worlds — one commit at a time.
          </p>

          <button
            class="prisma-fade-up group inline-flex items-center gap-2 self-start rounded-full py-1 pl-5 pr-1 text-sm font-medium text-black transition-all hover:gap-3 sm:text-base"
            class:prisma-visible={mounted}
            style="background-color: #E1E0CC; transition-delay: 700ms;"
            on:click={() => dispatch('navigate', 'about')}
          >
            About me
            <span
              class="flex h-9 w-9 items-center justify-center rounded-full bg-black transition-transform group-hover:scale-110 sm:h-10 sm:w-10"
            >
              <svg
                xmlns="http://www.w3.org/2000/svg"
                viewBox="0 0 24 24"
                fill="none"
                stroke="currentColor"
                stroke-width="2"
                stroke-linecap="round"
                stroke-linejoin="round"
                class="h-4 w-4"
                style="color: #E1E0CC;"
              >
                <path d="M5 12h14" />
                <path d="m12 5 7 7-7 7" />
              </svg>
            </span>
          </button>

        </div>
      </div>
    </div>
  </div>
</section>

<style>
  .prisma-fade-up {
    opacity: 0;
    transform: translateY(20px);
    transition:
      opacity 0.8s cubic-bezier(0.16, 1, 0.3, 1),
      transform 0.8s cubic-bezier(0.16, 1, 0.3, 1),
      gap 0.3s ease;
    will-change: opacity, transform;
  }
  .prisma-visible {
    opacity: 1;
    transform: translateY(0);
  }
</style>
