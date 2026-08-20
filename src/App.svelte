<script lang="ts">
  import { fade, fly } from 'svelte/transition';
  import { onMount } from 'svelte';
  import Navbar from './components/Navbar.svelte';
  import Home from './components/Home.svelte';
  import About from './components/About.svelte';
  import Projects from './components/Projects.svelte';

  let currentSection = 'home';

  function handleNavigate(event: CustomEvent<string>) {
    currentSection = event.detail;
  }

  // --- Custom Cursor Logic ---
  let cursorEl: HTMLDivElement;
  let mouseX = 0;
  let mouseY = 0;
  let isHovering = false;

  onMount(() => {
    let cX = window.innerWidth / 2;
    let cY = window.innerHeight / 2;

    const handleMouseMove = (e: MouseEvent) => {
        mouseX = e.clientX;
        mouseY = e.clientY;
        const target = e.target as HTMLElement;
        isHovering = !!target.closest('a, button, .cursor-pointer, .hover-glow, .smooth-btn, [role="button"]');
    };

    let animFrame: number;
    const animate = () => {
        cX += (mouseX - cX) * 0.15;
        cY += (mouseY - cY) * 0.15;
        if (cursorEl) {
            cursorEl.style.transform = `translate3d(${cX}px, ${cY}px, 0)`;
        }
        animFrame = requestAnimationFrame(animate);
    };

    window.addEventListener('mousemove', handleMouseMove);
    animFrame = requestAnimationFrame(animate);

    return () => {
        window.removeEventListener('mousemove', handleMouseMove);
        cancelAnimationFrame(animFrame);
    };
  });
</script>

<!-- Tiny mouse follower -->
<div 
    bind:this={cursorEl}
    class="fixed top-0 left-0 pointer-events-none z-[99999]"
    style="will-change: transform;"
>
    <div class="relative transition-transform duration-300 ease-out" style="transform: translate(-50%, -50%) scale({isHovering ? 1.5 : 1});">
        <div class="absolute inset-[-3px] rounded-full border border-blue-400/30 transition-all duration-300 {isHovering ? 'border-blue-400/60 scale-125' : ''}"></div>
        <div class="w-[40px] h-[40px] rounded-full overflow-hidden border-2 border-white/30 shadow-[0_0_15px_rgba(59,130,246,0.5)] transition-all duration-300">
            <img src="/loge.png" alt="" class="w-full h-full object-cover">
        </div>
    </div>
</div>

<div class="fixed inset-0 pointer-events-none -z-10 bg-[#0a0a0f] overflow-hidden">
    <!-- Animated Glowing Blobs - Dark Theme -->
    <div class="absolute top-0 left-0 w-full h-full overflow-hidden opacity-50">
        <!-- Blue blob top-left -->
        <div class="absolute top-[-15%] left-[-10%] w-[45%] h-[45%] rounded-full blur-[120px] animate-blob" style="background: #1e40af;"></div>
        <!-- Cyan blob bottom-left -->
        <div class="absolute bottom-[-15%] left-[-5%] w-[40%] h-[40%] rounded-full blur-[100px] animate-blob" style="background: linear-gradient(135deg, #0891b2, #2563eb); animation-delay: 2s;"></div>
        <!-- Blue blob top-right -->
        <div class="absolute top-[10%] right-[-10%] w-[40%] h-[40%] rounded-full blur-[110px] animate-blob" style="background: #3b82f6; animation-delay: 4s;"></div>
        <!-- Indigo blob bottom-right -->
        <div class="absolute bottom-[5%] right-[5%] w-[25%] h-[25%] rounded-full blur-[80px] animate-blob" style="background: #6366f1; animation-delay: 6s;"></div>
    </div>
    <!-- Grid Overlay -->
    <div class="absolute inset-0 opacity-[0.04]" style="background-image: linear-gradient(rgba(255,255,255,0.5) 1px, transparent 1px), linear-gradient(90deg, rgba(255,255,255,0.5) 1px, transparent 1px); background-size: 60px 60px;"></div>
    <!-- Watermark -->
    <div class="absolute top-1/2 left-1/2 transform -translate-x-1/2 -translate-y-1/2 text-[28vw] font-heading font-black text-white/[0.02] select-none whitespace-nowrap">OUSSAMA</div>
</div>

<!-- Main App Container -->
<main class="h-screen w-screen overflow-hidden flex flex-col relative font-sans">
    <Navbar {currentSection} on:navigate={handleNavigate} />
    
    <div class="flex-1 relative w-full h-full overflow-hidden">
        {#key currentSection}
            <div class="absolute inset-0 w-full h-full" in:fly={{ y: 30, duration: 400, delay: 200 }} out:fade={{ duration: 200 }}>
                {#if currentSection === 'home'}
                    <Home />
                {:else if currentSection === 'about'}
                    <About />
                {:else if currentSection === 'projects'}
                    <Projects />
                {/if}
            </div>
        {/key}
    </div>
</main>
