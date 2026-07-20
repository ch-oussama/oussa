<script lang="ts">
    const projects = [
        {
            id: '01',
            title: 'Doora Taxi App',
            desc: 'App icon and brand identity design for Doora, a modern ride-hailing app. Clean, bold identity built for mobile-first recognition.',
            tags: ['UI/UX', 'App Icon', 'Branding'],
            img: '/doora_apps.png',
            dark: true,
            icon: 'fa-solid fa-mobile-screen'
        },
        {
            id: '02',
            title: 'Duobix Studio',
            desc: 'Full brand identity and social media assets for Duobix Studio — a creative agency offering design, build & launch services.',
            tags: ['Branding', 'Social Media', 'Arabic'],
            img: '/lb.png',
            dark: false,
            icon: 'fa-solid fa-fingerprint'
        },
        {
            id: '03',
            title: 'DEER Clothing',
            desc: 'Minimal Arabic streetwear brand identity. Logo, color palette, and mockup visuals for DEER Arabic Clothes.',
            tags: ['Fashion', 'Branding', 'Logo'],
            img: '/larkdear_brandd.png',
            dark: false,
            icon: 'fa-solid fa-shirt'
        },
        {
            id: '04',
            title: 'Social Media Designs',
            desc: 'High-impact social media visuals for multiple brands — iFruit beverage, SABR clothing drop, and Hamoud Boualem Limonade.',
            tags: ['Social Media', 'Marketing', 'Poster'],
            img: '/asocial_msse.png',
            dark: true,
            icon: 'fa-solid fa-bullhorn'
        },
        {
            id: '05',
            title: 'YouTube Thumbnails',
            desc: 'High-CTR thumbnail designs with strong contrast, cinematic compositing, and visual storytelling to maximize click-through rates.',
            tags: ['Thumbnail', 'YouTube', 'Compositing'],
            img: '/larken_info_2.png',
            dark: false,
            icon: 'fa-brands fa-youtube'
        },
        {
            id: '06',
            title: 'Brit — Logo Design',
            desc: 'Clean, modern logo mark for Brit — a smart tech brand. Minimalist wordmark with strong brand character and futuristic aesthetic.',
            tags: ['Logo', 'Brand Identity', 'Tech'],
            img: '/brite.png',
            dark: true,
            icon: 'fa-solid fa-pen-nib'
        }
    ];
</script>

<div class="h-full w-full overflow-y-auto px-[5%] py-[20px] custom-scrollbar z-10 relative mt-20">
    <div class="max-w-[1200px] mx-auto pb-[100px] pt-6">
        <div class="mb-12 animate-slide-up">
            <div class="text-[10px] text-blue-400 font-bold tracking-[3px] mb-2 uppercase font-mono drop-shadow-[0_0_5px_rgba(59,130,246,0.3)]">[ CREATIVE_WORK ]</div>
            <h2 class="font-heading text-4xl lg:text-5xl uppercase tracking-tighter text-white">SELECTED<br>PROJECTS</h2>
        </div>

        <div class="grid grid-cols-1 md:grid-cols-2 gap-5">
            {#each projects as p, i}
                <div class="relative overflow-hidden group rounded-3xl border border-white/8 bg-white/3 backdrop-blur-sm flex flex-col justify-between min-h-[320px] cursor-pointer transition-all duration-500 hover:border-blue-500/30 hover:shadow-[0_0_40px_rgba(59,130,246,0.15)] hover:-translate-y-2 tilt-card stagger-item" style="animation-delay: {0.1 * (i + 1)}s;">
                    <div class="absolute inset-0 z-0">
                        <img src={p.img} alt={p.title} class="w-full h-full object-cover transition-transform duration-700 group-hover:scale-110">
                        <div class="absolute inset-0 bg-gradient-to-t from-[#0a0a0f] via-[#0a0a0f]/70 to-[#0a0a0f]/50 group-hover:from-[#0a0a0f]/95 group-hover:via-[#0a0a0f]/40 group-hover:to-[#0a0a0f]/10 transition-all duration-500"></div>
                    </div>
                    <div class="relative z-10 p-6 sm:p-8 h-full flex flex-col justify-between">
                        <div class="flex justify-between items-start">
                            <div class="text-[10px] font-mono font-bold text-white/40 tracking-widest">{p.id}</div>
                            <div class="w-10 h-10 rounded-full border border-white/20 bg-white/10 backdrop-blur-md flex items-center justify-center text-white group-hover:border-blue-400 group-hover:text-blue-400 group-hover:shadow-[0_0_15px_rgba(59,130,246,0.3)] group-hover:scale-110 transition-all duration-300">
                                <i class="{p.icon} text-[14px]"></i>
                            </div>
                        </div>
                        <div>
                            <h3 class="text-xl sm:text-2xl font-bold tracking-tight mb-2 text-white drop-shadow-lg">{p.title}</h3>
                            <p class="text-[12px] text-white/60 leading-relaxed mb-4 max-w-[90%] opacity-0 group-hover:opacity-100 transition-all duration-500 translate-y-4 group-hover:translate-y-0 delay-75">
                                {p.desc}
                            </p>
                            <div class="flex flex-wrap gap-2">
                                {#each p.tags as tag}
                                    <span class="text-[10px] font-bold px-3 py-1.5 rounded-full bg-white/10 backdrop-blur-md border border-white/10 text-white/80 group-hover:bg-blue-500/20 group-hover:border-blue-400/30 group-hover:text-blue-300 transition-all duration-300 hover:scale-105">
                                        {tag}
                                    </span>
                                {/each}
                            </div>
                        </div>
                    </div>
                    <div class="absolute inset-0 z-20 opacity-0 group-hover:opacity-100 transition-opacity duration-500 pointer-events-none">
                        <div class="absolute inset-0 bg-gradient-to-r from-transparent via-white/5 to-transparent transform -translate-x-full group-hover:translate-x-full transition-transform duration-1000"></div>
                    </div>
                </div>
            {/each}
        </div>
    </div>
</div>
