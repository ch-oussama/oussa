<script lang="ts">
    import { onMount, createEventDispatcher } from 'svelte';
    const dispatch = createEventDispatcher();

    let ready = false;
    let revealImg: HTMLDivElement;
    let maskSize = 260;

    onMount(() => {
        // Trigger animations after a short tick
        setTimeout(() => {
            ready = true;
        }, 50);

        // Responsive mask size
        const updateMaskSize = () => {
            if (window.innerWidth < 480) maskSize = 120;
            else if (window.innerWidth < 720) maskSize = 160;
            else maskSize = 260;
        };
        updateMaskSize();
        window.addEventListener('resize', updateMaskSize);

        const handleMove = (e: MouseEvent | TouchEvent) => {
            if (!revealImg) return;
            const rect = revealImg.getBoundingClientRect();
            let clientX, clientY;
            if (e.type.includes('touch')) {
                clientX = (e as TouchEvent).touches[0].clientX;
                clientY = (e as TouchEvent).touches[0].clientY;
            } else {
                clientX = (e as MouseEvent).clientX;
                clientY = (e as MouseEvent).clientY;
            }
            
            const x = clientX - rect.left;
            const y = clientY - rect.top;

            const mask = `radial-gradient(circle ${maskSize}px at ${x}px ${y}px, #fff 0%, #fff 40%, rgba(255,255,255,0.75) 60%, rgba(255,255,255,0.4) 75%, rgba(255,255,255,0.12) 88%, transparent 100%)`;
            
            revealImg.style.webkitMaskImage = mask;
            revealImg.style.maskImage = mask;
        };

        window.addEventListener('mousemove', handleMove);
        window.addEventListener('touchmove', handleMove, { passive: true });

        return () => {
            window.removeEventListener('resize', updateMaskSize);
            window.removeEventListener('mousemove', handleMove);
            window.removeEventListener('touchmove', handleMove);
        };
    });

    function copyUid() {
        navigator.clipboard.writeText("1352298288342372443");
        // Could add a toast here, simple alert for now
        alert("UID copied to clipboard!");
    }
</script>

<svelte:head>
    <link href="https://db.onlinewebfonts.com/c/b1314443e183d1cdd77049077c46facc?family=Orbitron-Medium" rel="stylesheet" type="text/css"/>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin="anonymous">
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600&display=swap" rel="stylesheet">
    <title>Oussa | Senior Engineer</title>
</svelte:head>

<main class="hero cyber-ronin">
    <div class="hero-base-img hero-image-animate" style="background-image: url('https://images.higgs.ai/?default=1&output=webp&url=https%3A%2F%2Fd8j0ntlcm91z4.cloudfront.net%2Fuser_38xzZboKViGWJOttwIXH07lWA1P%2Fhf_20260831_115955_2a9adb39-5e9b-4ced-96e2-6900eabe3de9.png&w=1920&q=85')"></div>
    <div bind:this={revealImg} class="hero-reveal-img" id="reveal-img" style="background-image: url('https://images.higgs.ai/?default=1&output=webp&url=https%3A%2F%2Fd8j0ntlcm91z4.cloudfront.net%2Fuser_38xzZboKViGWJOttwIXH07lWA1P%2Fhf_20260831_123709_183f0065-efb2-4bb2-a849-13aaa5af2f3f.png&w=1920&q=85')"></div>

    <div class="hero-ui">
        <!-- left side -->
        <div class="hero-left">
            <div class="text-[11px] font-mono text-white/60 tracking-widest mb-6 flex items-center gap-2 pointer-events-auto cursor-pointer hover:text-white transition-colors" on:click={copyUid}>
                DISCORD_UID: <span class="text-white font-bold">1352298288342372443</span> 
                <i class="fa-regular fa-copy"></i>
            </div>
            
            <h1 class="heading-font text-[9vw] md:text-[5vw] leading-[0.9] text-white uppercase drop-shadow-2xl pointer-events-auto">
                HI, I'M OUSSA<br/>
                SENIOR<br/>
                ENGINEER
            </h1>
            
            <div class="mt-8 flex items-center gap-2 text-white/80 text-[13px] font-medium pointer-events-auto">
                <span class="text-white font-bold">Founder of Jannah</span>
                <span class="w-1 h-1 rounded-full bg-white/50"></span>
                <span>Artificial Intelligence Student</span>
            </div>
            
            <div class="mt-8 flex items-center gap-4 pointer-events-auto">
                <button class="bg-white text-black px-6 py-3 rounded-full text-sm font-bold flex items-center gap-2 hover:scale-105 transition-transform" on:click={() => dispatch('navigate', 'projects')}>
                    View Projects <i class="fa-solid fa-arrow-right"></i>
                </button>
                <button class="bg-white/10 backdrop-blur-md border border-white/20 text-white px-6 py-3 rounded-full text-sm font-bold flex items-center gap-2 hover:bg-white/20 transition-colors" on:click={() => dispatch('navigate', 'contact')}>
                    <i class="fa-regular fa-paper-plane"></i> Get In Touch
                </button>
            </div>
        </div>

        <!-- right side -->
        <div class="hero-right pointer-events-auto text-right">
            <div class="text-[11px] font-bold tracking-[0.2em] text-white uppercase mb-12">
                OPEN TO WORK & COLLABORATION
            </div>
            
            <div class="flex gap-16 text-left mb-16">
                <div>
                    <div class="text-[10px] font-bold tracking-[0.15em] text-white/50 mb-4">CORE_EXPERTISE</div>
                    <ul class="flex flex-col gap-3 text-[14px] font-medium text-white">
                        <li>Next.js / TypeScript</li>
                        <li>Node.js Backend</li>
                        <li>System Architecture</li>
                    </ul>
                </div>
                <div>
                    <div class="text-[10px] font-bold tracking-[0.15em] text-white/50 mb-4">PRODUCT_FOCUS</div>
                    <ul class="flex flex-col gap-3 text-[14px] font-medium text-white">
                        <li>Scalable Infrastructure</li>
                        <li>User-Centric UI/UX</li>
                        <li>Performance Tuning</li>
                    </ul>
                </div>
            </div>
            
            <div class="flex flex-col items-end gap-5 w-full max-w-[400px]">
                <div class="text-[10px] font-bold tracking-[0.2em] text-white/50">
                    [ CONNECT ]
                </div>
                <div class="flex gap-5 text-white text-xl">
                    <a href="#" class="hover:text-white/70 transition-colors hover:scale-110 transform duration-200"><i class="fa-brands fa-github"></i></a>
                    <a href="#" class="hover:text-white/70 transition-colors hover:scale-110 transform duration-200"><i class="fa-brands fa-whatsapp"></i></a>
                    <a href="#" class="hover:text-white/70 transition-colors hover:scale-110 transform duration-200"><i class="fa-regular fa-envelope"></i></a>
                    <a href="#" class="hover:text-white/70 transition-colors hover:scale-110 transform duration-200"><i class="fa-brands fa-instagram"></i></a>
                    <a href="#" class="hover:text-white/70 transition-colors hover:scale-110 transform duration-200"><i class="fa-brands fa-snapchat"></i></a>
                    <a href="#" class="hover:text-white/70 transition-colors hover:scale-110 transform duration-200"><i class="fa-brands fa-discord"></i></a>
                </div>
                <p class="text-[12px] text-white/60 leading-relaxed text-right mt-2">
                    Engineering digital solutions with a focus on clean code, scalability, and state-of-the-art technology integrations.
                </p>
            </div>
        </div>
    </div>
</main>

<style>
    .cyber-ronin {
        --cream: #FBDBAF;
        --muted: rgba(251, 219, 175, 0.72);
        --label: rgba(251, 219, 175, 0.48);
        --orange: #E07020;
        --card: rgba(10, 8, 7, 0.58);
        
        font-family: 'Inter', system-ui, -apple-system, sans-serif;
        background: #FF7F00;
        color: var(--cream);
    }
    
    .cyber-ronin .heading-font {
        font-family: 'Orbitron-Medium', 'Arial Narrow', sans-serif;
        font-weight: 400;
        letter-spacing: 0.02em;
    }

    .hero {
        position: relative;
        width: 100%;
        height: 100%;
        overflow: hidden;
        background: #FF7F00;
    }

    .hero-base-img, .hero-reveal-img {
        position: absolute;
        inset: 0;
        background-size: cover;
        background-position: center center;
    }

    .hero-base-img {
        z-index: 1;
    }

    .hero-reveal-img {
        z-index: 2;
        pointer-events: none;
        -webkit-mask-image: radial-gradient(circle 0px at -999px -999px, #fff, transparent);
        mask-image: radial-gradient(circle 0px at -999px -999px, #fff, transparent);
    }

    .hero-ui {
        position: absolute;
        inset: 0;
        z-index: 8;
        display: grid;
        grid-template-columns: 1fr 1fr;
        grid-template-rows: auto 1fr auto;
        padding: 36px 44px 40px;
        pointer-events: none;
    }

    .hero-left {
        grid-column: 1;
        grid-row: 1 / -1;
        display: flex;
        flex-direction: column;
        justify-content: flex-end;
        align-items: flex-start;
        min-width: 0;
        padding-bottom: 20px;
    }

    .hero-right {
        grid-column: 2;
        grid-row: 1 / -1;
        display: flex;
        flex-direction: column;
        justify-content: flex-end;
        align-items: flex-end;
        padding-bottom: 20px;
    }

    .pull-line {
        display: block;
    }

    .pull-word {
        display: inline-block;
        opacity: 0;
        transform: translateY(20px);
    }
    .pull-word:not(:last-child) {
        margin-right: 0.3em;
    }
    
    /* Animation triggers for words-pull-up */
    .words-pull-up.words-visible .pull-word {
        animation: wordPullUp 0.55s ease forwards;
    }
    
    /* Animation triggers for h2 words specifically */
    .product-card h2.is-visible .pull-word {
        animation: wordPullUp 0.55s ease forwards;
    }

    .hero-copy p {
        margin-top: 18px;
        max-width: 340px;
        font-size: 13.5px;
        font-weight: 400;
        line-height: 1.55;
        color: var(--muted);
    }

    .icon-row {
        display: flex;
        gap: 10px;
        margin-top: 22px;
    }

    .icon-btn {
        width: 38px;
        height: 38px;
        border-radius: 50%;
        border: 1px solid rgba(251,219,175,0.55);
        background: transparent;
        color: var(--cream);
        display: grid;
        place-items: center;
        transition: background 0.25s ease, border-color 0.25s ease;
        cursor: pointer;
    }

    .icon-btn:hover {
        background: rgba(251,219,175,0.1);
        border-color: var(--cream);
    }

    .product-card {
        pointer-events: auto;
        width: min(320px, 100%);
        display: grid;
        grid-template-columns: 86px 1fr;
        grid-template-rows: auto auto;
        gap: 10px 14px;
        padding: 12px;
        border-radius: 18px;
        background: var(--card);
        -webkit-backdrop-filter: blur(18px);
        backdrop-filter: blur(18px);
        border: 1px solid rgba(251,219,175,0.08);
        box-shadow: 0 18px 50px rgba(0,0,0,0.28);
    }

    .product-thumb {
        grid-column: 1;
        grid-row: 1 / -1;
        width: 86px;
        min-height: 86px;
        border-radius: 12px;
        overflow: hidden;
        background-size: cover;
        background-position: center;
    }

    .product-body {
        grid-column: 2;
        grid-row: 1;
    }

    .product-card h2 {
        font-size: 11px;
        line-height: 1.25;
        text-transform: uppercase;
        color: var(--cream);
    }

    .product-card p {
        margin-top: 6px;
        font-size: 11.5px;
        line-height: 1.45;
        color: var(--muted);
    }

    .cart-btn {
        grid-column: 2;
        grid-row: 2;
        justify-self: start;
        align-self: end;
        border: 1px solid var(--cream);
        border-radius: 999px;
        padding: 7px 16px;
        font-size: 12px;
        font-weight: 500;
        color: var(--cream);
        background: transparent;
        transition: border-color 0.2s ease, color 0.2s ease, transform 0.2s ease;
        cursor: pointer;
    }

    .cart-btn:hover {
        transform: translateY(-1px);
    }

    .hero-page {
        position: absolute;
        top: 36px;
        right: 44px;
        font-size: 13px;
        letter-spacing: 0.08em;
        color: rgba(251,219,175,0.78);
    }

    .specs {
        grid-column: 2;
        grid-row: 3;
        justify-self: end;
        pointer-events: auto;
        width: min(340px, 100%);
    }

    .specs h3 {
        font-size: 11px;
        text-transform: uppercase;
        margin-bottom: 14px;
        letter-spacing: 0.12em;
    }

    .spec-row {
        display: flex;
        justify-content: space-between;
        align-items: baseline;
        gap: 16px;
        padding: 7px 0;
    }
    
    .spec-row + .spec-row {
        border-top: 1px solid rgba(251,219,175,0.12);
    }

    .spec-label {
        font-size: 11px;
        letter-spacing: 0.08em;
        text-transform: uppercase;
        color: var(--label);
    }

    .spec-value {
        font-size: 12.5px;
        color: var(--cream);
        text-align: right;
        white-space: nowrap;
    }

    .hero-image-animate {
        animation: heroImageIn 1.2s cubic-bezier(0.25,0.46,0.45,0.94) forwards;
        animation-delay: 0.15s;
        opacity: 0;
    }

    .fade-up-reveal {
        opacity: 0;
        transform: translateY(14px);
        filter: blur(8px);
    }

    .fade-up-reveal.is-visible {
        animation: fadeUp 0.7s ease forwards;
    }

    @keyframes heroImageIn {
        from { opacity: 0; transform: scale(1.18); }
        to { opacity: 1; transform: scale(1); }
    }

    @keyframes wordPullUp {
        from { opacity: 0; transform: translateY(20px); }
        to { opacity: 1; transform: translateY(0); }
    }

    @keyframes fadeUp {
        from { opacity: 0; transform: translateY(14px); filter: blur(8px); }
        to { opacity: 1; transform: translateY(0); filter: blur(0); }
    }

    @media (prefers-reduced-motion: reduce) {
        * {
            animation: none !important;
            opacity: 1 !important;
            transform: none !important;
            filter: none !important;
        }
    }

    /* Responsive Breakpoints */
    @media (max-width: 1024px) {
        .hero-ui { padding: 32px 28px 36px; }
    }

    @media (max-width: 900px) {
        .hero-ui { padding: 28px 22px 28px; }
        .hero-base-img, .hero-reveal-img { background-position: 40% center; }
    }

    @media (max-width: 768px) {
        .hero-ui {
            grid-template-columns: 1fr;
            grid-template-rows: auto 1fr auto;
            padding: max(24px, env(safe-area-inset-top)) max(20px, env(safe-area-inset-right)) max(28px, env(safe-area-inset-bottom)) max(20px, env(safe-area-inset-left));
        }
        .hero-left { grid-row: 1; gap: 24px; padding-bottom: 40px; }
        .hero-right { grid-row: 2; align-items: flex-start; text-align: left; }
        .hero-right .text-right { text-align: left; }
        .hero-right .items-end { items-align: flex-start; }
        .hero-base-img, .hero-reveal-img { background-position: 40% center; }
    }

    @media (max-width: 720px) {
        .hero {
            height: 100%;
            min-height: 100%;
        }
        .hero-ui {
            position: relative;
            display: flex;
            flex-direction: column;
            min-height: 100%;
            justify-content: flex-start;
            gap: 28px;
            padding: max(22px, env(safe-area-inset-top)) max(18px, env(safe-area-inset-right)) max(24px, env(safe-area-inset-bottom)) max(18px, env(safe-area-inset-left));
        }
        .hero-left {
            width: 100%;
            justify-content: flex-start;
        }
        .hero-base-img, .hero-reveal-img {
            position: absolute;
            background-position: 40% center;
        }
    }

    @media (max-width: 480px) {
        .hero-ui {
            gap: 22px;
            padding: max(18px, env(safe-area-inset-top)) max(16px, env(safe-area-inset-right)) max(20px, env(safe-area-inset-bottom)) max(16px, env(safe-area-inset-left));
        }
        .hero-left { gap: 22px; }
    }

    @media (max-width: 360px) {
        .cyber-ronin h1 { font-size: 1rem; }
        .product-card { grid-template-columns: 64px 1fr; }
        .product-thumb { width: 64px; min-height: 64px; }
    }
</style>
