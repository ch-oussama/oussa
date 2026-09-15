<script lang="ts">
    import { onMount } from 'svelte';
    import TempleNightScene from './TempleNightScene.svelte';

    let showScene = false;
    let activeFilter = 'all';
    let mounted = false;

    onMount(() => {
        setTimeout(() => {
            showScene = true;
        }, 300);
        requestAnimationFrame(() => {
            mounted = true;
        });
    });

    const categories = [
        { id: 'all', label: 'All Projects', icon: 'fa-solid fa-layer-group' },
        { id: 'games', label: 'Games & FiveM', icon: 'fa-solid fa-gamepad' },
        { id: 'web', label: 'Web & App', icon: 'fa-solid fa-laptop-code' },
        { id: 'tools', label: 'Tools', icon: 'fa-solid fa-wrench' },
    ];

    const projects = [
        {
            title: 'Arab Mediators',
            subtitle: 'Escrow & Trade Mediation Web Hub',
            desc: 'Official digital escrow and trusted trade mediation platform on Discord with live community statistics and dynamic fee calculation workflows.',
            tags: ['Next.js 15', 'TypeScript', 'Discord API', 'Tailwind'],
            img: '/doora_apps.png', // Fallback image path
            category: 'web',
            link: '#',
        },
        {
            title: 'Oufa',
            subtitle: 'Graphic Design Engineering Showcase',
            desc: 'Bespoke digital portfolio featuring custom audio introduction players, atmospheric themes, and interactive milestones.',
            tags: ['Next.js', 'TypeScript', 'Web Audio API', 'Canvas'],
            img: '/lb.png',
            category: 'web',
            link: '#',
        },
        {
            title: 'NoPixel Inspired Core',
            subtitle: 'FiveM Roleplay Framework',
            desc: 'A highly optimized, custom roleplay framework built for FiveM. Features custom inventory systems, dynamic economy, and real-time database syncing.',
            tags: ['Lua', 'FiveM', 'SQL', 'Node.js'],
            img: '/larkdear_brandd.png',
            category: 'games',
            link: '#',
        },
        {
            title: 'Jannah DevTools',
            subtitle: 'CLI Utility Toolkit',
            desc: 'A suite of command-line tools built for rapid game development workflows. Automates asset packing, texture compression, and deployment.',
            tags: ['Rust', 'CLI', 'Automation'],
            img: '/asocial_msse.png',
            category: 'tools',
            link: '#',
        },
        {
            title: 'Shadowfall Arena',
            subtitle: 'Multiplayer FPS Prototype',
            desc: 'A fast-paced multiplayer FPS prototype built in Unreal Engine 5 with custom C++ movement mechanics and dedicated server architecture.',
            tags: ['Unreal Engine 5', 'C++', 'Multiplayer'],
            img: '/larken_info_2.png',
            category: 'games',
            link: '#',
        },
        {
            title: 'Server Analytics Dashboard',
            subtitle: 'Real-time Game Server Monitoring',
            desc: 'A real-time dashboard for monitoring game server health, active players, and resource consumption with visual graphs and alerts.',
            tags: ['React', 'WebSocket', 'Express', 'Charts'],
            img: '/brite.png',
            category: 'tools',
            link: '#',
        }
    ];

    $: filteredProjects = activeFilter === 'all'
        ? projects
        : projects.filter(p => p.category === activeFilter);

    $: projectCount = (cat: string) => cat === 'all'
        ? projects.length
        : projects.filter(p => p.category === cat).length;
</script>

<div class="projects-page h-full w-full overflow-y-auto custom-scrollbar z-10 relative mt-20">
    <!-- Background -->
    <div class="pointer-events-none fixed inset-0 z-0 overflow-hidden transition-opacity duration-1000 {showScene ? 'opacity-100' : 'opacity-0'}">
        {#if showScene}
            <TempleNightScene variant="temple-night" />
        {/if}
    </div>
    <div class="pointer-events-none fixed inset-0 z-[1] bg-gradient-to-b from-[#0E0C09]/75 via-[#0E0C09]/60 to-[#0E0C09]/90"></div>

    <div class="relative z-[2] max-w-[1350px] mx-auto px-6 md:px-10 pb-[120px] pt-8">

        <!-- Header -->
        <div class="text-center mb-14 proj-fade" class:proj-visible={mounted} style="transition-delay: 0.1s;">
            <div class="text-[10px] font-mono font-bold tracking-[0.3em] text-[#D6C9A8] uppercase mb-4">PORTFOLIO & WORKS</div>
            <h2 class="font-heading text-[clamp(2rem,6vw,3.5rem)] uppercase tracking-[-2px] text-white leading-[0.95] mb-5">
                FEATURED<br>PROJECTS
            </h2>
            <p class="text-[13px] text-white/50 max-w-lg mx-auto leading-relaxed">
                A curated collection of production web platforms, bespoke creative systems, and interactive digital products.
            </p>
        </div>

        <!-- Category Filters -->
        <div class="flex flex-wrap justify-center gap-3 mb-12 proj-fade" class:proj-visible={mounted} style="transition-delay: 0.2s;">
            {#each categories as cat}
                <button
                    class="filter-btn"
                    class:active={activeFilter === cat.id}
                    onclick={() => activeFilter = cat.id}
                >
                    <i class="{cat.icon} text-[11px] opacity-70"></i> {cat.label} <span class="filter-count">({projectCount(cat.id)})</span>
                </button>
            {/each}
        </div>

        <!-- Projects Grid -->
        <div class="grid grid-cols-1 md:grid-cols-2 gap-7">
            {#each filteredProjects as p, i (p.title)}
                <article class="project-card proj-fade" class:proj-visible={mounted} style="transition-delay: {0.2 + 0.1 * (i + 1)}s;">
                    <!-- Image Preview -->
                    <div class="card-image-wrap">
                        <img src={p.img} alt={p.title} class="card-image" loading="lazy" />
                        <div class="card-image-overlay"></div>
                    </div>

                    <!-- Content -->
                    <div class="card-content">
                        <h3 class="card-title">{p.title}</h3>
                        <div class="card-subtitle">{p.subtitle}</div>
                        <p class="card-desc">{p.desc}</p>

                        <!-- Tags -->
                        <div class="card-tags">
                            {#each p.tags as tag}
                                <span class="card-tag">{tag}</span>
                            {/each}
                        </div>

                        <!-- Visit Button -->
                        <a href={p.link} target="_blank" rel="noopener noreferrer" class="visit-btn">
                            Visit Website <i class="fa-solid fa-arrow-up-right-from-square text-[10px]"></i>
                        </a>
                    </div>
                </article>
            {/each}
        </div>

    </div>
</div>

<style>
    .projects-page {
        font-family: 'Inter', system-ui, -apple-system, sans-serif;
    }

    /* --- Fade In Animation --- */
    .proj-fade {
        opacity: 0;
        transform: translateY(24px);
        transition: opacity 0.7s cubic-bezier(0.16, 1, 0.3, 1), transform 0.7s cubic-bezier(0.16, 1, 0.3, 1);
    }
    .proj-visible {
        opacity: 1;
        transform: translateY(0);
    }

    /* --- Filter Buttons --- */
    .filter-btn {
        display: inline-flex;
        align-items: center;
        gap: 4px;
        padding: 8px 20px;
        border-radius: 999px;
        border: 1px solid rgba(255, 255, 255, 0.12);
        background: rgba(255, 255, 255, 0.04);
        color: rgba(255, 255, 255, 0.6);
        font-size: 12px;
        font-weight: 600;
        cursor: pointer;
        transition: all 0.3s ease;
        backdrop-filter: blur(8px);
    }
    .filter-btn:hover {
        border-color: rgba(225, 224, 204, 0.3);
        color: white;
        background: rgba(255, 255, 255, 0.08);
    }
    .filter-btn.active {
        background: rgba(225, 224, 204, 0.12);
        border-color: rgba(225, 224, 204, 0.35);
        color: #E1E0CC;
        box-shadow: 0 0 20px rgba(212, 163, 115, 0.15);
    }
    .filter-count {
        font-size: 10px;
        opacity: 0.5;
        font-weight: 400;
    }

    /* --- Project Card --- */
    .project-card {
        background: rgba(20, 18, 15, 0.6);
        border: 1px solid rgba(255, 255, 255, 0.07);
        border-radius: 20px;
        overflow: hidden;
        transition: all 0.5s cubic-bezier(0.16, 1, 0.3, 1);
        backdrop-filter: blur(12px);
    }
    .project-card:hover {
        border-color: rgba(212, 163, 115, 0.25);
        box-shadow: 0 20px 60px rgba(0, 0, 0, 0.3), 0 0 40px rgba(212, 163, 115, 0.08);
        transform: translateY(-6px);
    }

    /* --- Card Image --- */
    .card-image-wrap {
        position: relative;
        width: 100%;
        aspect-ratio: 16 / 9;
        overflow: hidden;
        border-bottom: 1px solid rgba(255, 255, 255, 0.05);
    }
    .card-image {
        width: 100%;
        height: 100%;
        object-fit: cover;
        transition: transform 0.7s cubic-bezier(0.16, 1, 0.3, 1);
    }
    .project-card:hover .card-image {
        transform: scale(1.06);
    }
    .card-image-overlay {
        position: absolute;
        inset: 0;
        background: linear-gradient(to top, rgba(20, 18, 15, 0.4) 0%, transparent 50%);
        pointer-events: none;
    }

    /* --- Card Content --- */
    .card-content {
        padding: 24px 28px 28px;
    }
    .card-title {
        font-size: 20px;
        font-weight: 700;
        color: white;
        letter-spacing: -0.02em;
        margin-bottom: 4px;
    }
    .card-subtitle {
        font-size: 12px;
        color: rgba(225, 224, 204, 0.5);
        font-weight: 500;
        font-style: italic;
        margin-bottom: 12px;
    }
    .card-desc {
        font-size: 13px;
        line-height: 1.65;
        color: rgba(255, 255, 255, 0.45);
        margin-bottom: 18px;
    }

    /* --- Tags --- */
    .card-tags {
        display: flex;
        flex-wrap: wrap;
        gap: 6px;
        margin-bottom: 20px;
    }
    .card-tag {
        font-size: 10px;
        font-weight: 600;
        padding: 5px 12px;
        border-radius: 999px;
        border: 1px solid rgba(255, 255, 255, 0.1);
        background: rgba(255, 255, 255, 0.04);
        color: rgba(255, 255, 255, 0.55);
        transition: all 0.3s ease;
    }
    .project-card:hover .card-tag {
        border-color: rgba(212, 163, 115, 0.2);
        color: rgba(225, 224, 204, 0.7);
        background: rgba(212, 163, 115, 0.08);
    }

    /* --- Visit Button --- */
    .visit-btn {
        display: inline-flex;
        align-items: center;
        gap: 8px;
        padding: 10px 22px;
        border-radius: 999px;
        border: 1px solid rgba(225, 224, 204, 0.25);
        background: transparent;
        color: #E1E0CC;
        font-size: 12px;
        font-weight: 600;
        cursor: pointer;
        transition: all 0.3s ease;
        text-decoration: none;
    }
    .visit-btn:hover {
        background: rgba(225, 224, 204, 0.1);
        border-color: rgba(225, 224, 204, 0.5);
        box-shadow: 0 0 20px rgba(212, 163, 115, 0.15);
        transform: translateX(4px);
    }

    /* --- Responsive --- */
    @media (max-width: 768px) {
        .card-content {
            padding: 18px 20px 22px;
        }
        .card-title {
            font-size: 17px;
        }
        .card-desc {
            font-size: 12px;
        }
    }
</style>
