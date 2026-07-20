<script lang="ts">
    import { onMount } from 'svelte';

    let logLines: { time: string; level: string; levelColor: string; msg: string }[] = [
        { time: '20:24:01', level: 'WRN', levelColor: '#f59e0b', msg: '[Unity] Build optimization in progress' },
        { time: '20:23:45', level: 'INF', levelColor: '#38bdf8', msg: '[Unreal] Shader compilation complete' },
        { time: '20:22:12', level: 'OK',  levelColor: '#34d399', msg: '[Server] Game server connection STABLE' },
        { time: '20:21:05', level: 'SYS', levelColor: '#818cf8', msg: '[DevTools] Game engine processes active' },
        { time: '20:15:00', level: 'OK',  levelColor: '#34d399', msg: '[Portfolio] Runtime active - V3.0 deployed' },
        { time: '20:12:33', level: 'INF', levelColor: '#38bdf8', msg: '[Git] Pushed 14 commits to main' },
        { time: '20:10:00', level: 'OK',  levelColor: '#34d399', msg: '[Build] Compilation successful - 0 errors' },
    ];

    const activityGrid: number[][] = Array.from({ length: 16 }, (_, col) =>
        Array.from({ length: 7 }, (_, row) => {
            const seed = col * 7 + row;
            const r1 = Math.sin(seed * 9301 + 49297) * 100;
            const v = r1 - Math.floor(r1);
            return v > 0.3 ? v * 0.8 + 0.2 : 0.05;
        })
    );

    let memValue = '14.2 GB';
    let cpuValue = '12%';
    let netValue = '24ms';
    let cursorVisible = true;

    const codeSkills = [
        { name: 'C# / Unity', level: 95 },
        { name: 'C++ / Unreal', level: 88 },
        { name: 'JavaScript / TypeScript', level: 90 },
        { name: 'Python', level: 82 },
        { name: 'Rust / Godot', level: 75 },
        { name: 'SQL / NoSQL', level: 85 },
    ];

    const tools = [
        'Unity', 'Unreal Engine', 'Godot', 'Blender', 'Maya',
        'Photoshop', 'Substance Painter', 'VS Code', 'Git', 'Docker',
        'Firebase', 'AWS', 'Linux', 'Figma', 'Jira'
    ];

    const services = [
        {
            icon: 'fa-solid fa-gamepad',
            title: 'Game Development',
            desc: 'Building immersive games from concept to launch. Gameplay mechanics, level design, and player experience optimization across multiple platforms.',
            tags: ['Unity', 'Unreal', 'Godot']
        },
        {
            icon: 'fa-solid fa-code',
            title: 'Game Programming',
            desc: 'Writing clean, performant game code. AI systems, physics engines, networking, and shader programming for stunning visuals.',
            tags: ['C#', 'C++', 'GLSL']
        },
        {
            icon: 'fa-solid fa-shield-halved',
            title: 'Anti-Cheat Systems',
            desc: 'Implementing robust security measures for multiplayer games. Memory protection, server validation, and cheat detection algorithms.',
            tags: ['Security', 'Networking', 'Encryption']
        },
        {
            icon: 'fa-solid fa-server',
            title: 'Multiplayer & Backend',
            desc: 'Building scalable game servers, matchmaking systems, and real-time multiplayer infrastructure that handles thousands of concurrent players.',
            tags: ['Nodes', 'WebSocket', 'Redis']
        }
    ];

    const techStack = [
        { category: 'ENGINES', items: [
            { name: 'Unity 2D/3D', pct: 95 },
            { name: 'Unreal Engine 5', pct: 88 },
            { name: 'Godot 4', pct: 78 },
        ]},
        { category: 'LANGUAGES', items: [
            { name: 'C#', pct: 95 },
            { name: 'C++', pct: 88 },
            { name: 'GLSL / HLSL', pct: 80 },
            { name: 'Python', pct: 82 },
        ]},
        { category: 'TOOLS & ECOSYSTEM', items: [
            { name: 'Git & CI/CD', pct: 92 },
            { name: 'Docker & K8s', pct: 75 },
            { name: 'Firebase / AWS', pct: 85 },
        ]},
    ];

    onMount(() => {
        const interval = setInterval(() => {
            cursorVisible = !cursorVisible;
            const cpu = Math.floor(Math.random() * 8 + 9);
            const net = Math.floor(Math.random() * 10 + 20);
            cpuValue = `${cpu}%`;
            netValue = `${net}ms`;
        }, 1200);
        return () => clearInterval(interval);
    });
</script>

<div class="h-full w-full overflow-y-auto px-[4%] py-5 custom-scrollbar z-10 relative mt-20">
    <div class="max-w-[1100px] mx-auto pb-20">

        <!-- ══════════ HERO CARD ══════════ -->
        <div class="hero-card mb-3 p-8 sm:p-10 relative overflow-hidden group page-transition">
            <div class="absolute right-[-20px] bottom-[-30px] text-[140px] font-heading font-black select-none pointer-events-none watermark-text tracking-tighter">DEVELOPER</div>
            <div class="scan-overlay absolute inset-0 pointer-events-none opacity-[0.02]"></div>

            <div class="flex flex-col sm:flex-row gap-6 items-start sm:items-center mb-7 relative z-10">
                <div class="relative shrink-0 animate-scale-in">
                    <div class="avatar-glow absolute inset-0 rounded-full"></div>
                    <img src="/loge.png" alt="Oussama" class="relative w-[90px] h-[90px] sm:w-[100px] sm:h-[100px] rounded-full object-cover border-2 border-white/20 shadow-2xl hover:shadow-[0_0_20px_rgba(59,130,246,0.4)] transition-shadow duration-300">
                </div>
                <div class="animate-slide-up">
                    <div class="role-badge inline-flex items-center gap-2 px-3 py-1 rounded-full text-[9px] sm:text-[10px] font-bold tracking-[0.2em] uppercase mb-4">
                        <span class="w-1.5 h-1.5 rounded-full bg-blue-400 animate-pulse shadow-[0_0_6px_#3b82f6]"></span>
                        Game Developer &amp; Senior Engineer
                    </div>
                    <h1 class="text-[38px] sm:text-[54px] md:text-[64px] font-heading font-black uppercase tracking-[-2px] leading-none text-white">
                        OUSSAMA <span class="text-white/30 font-light">(JANNAH)</span>
                    </h1>
                </div>
            </div>

            <p class="text-[14px] sm:text-[15px] text-white/50 leading-relaxed mb-8 max-w-4xl relative z-10 font-light">
                Oussama is a passionate Game Developer & Senior Engineer from Algeria who builds mostly because he genuinely enjoys it.
                Alongside his passion for game development, he's also building
                <span class="text-blue-400 font-bold">Jannah</span>,
                an agency dedicated to creative solutions. Most of his time goes into building immersive game experiences, crafting game mechanics,
                and experimenting with new engines and frameworks for absolutely no reason other than "it sounded fun."
            </p>

            <div class="relative z-10 flex flex-wrap gap-3">
                <div class="status-badge inline-flex items-center gap-2 px-4 py-2 rounded-full text-[10px] font-bold tracking-wider">
                    <div class="w-2 h-2 bg-blue-400 rounded-full animate-pulse shadow-[0_0_6px_#3b82f6]"></div>
                    AVAILABLE_FOR_PROJECTS
                </div>
                <a href="/cv.pdf" target="_blank" class="dl-btn inline-flex items-center gap-2 px-5 py-2 rounded-full text-[10px] font-bold transition-all duration-300 magnetic-btn ripple hover-glow">
                    <i class="fa-solid fa-arrow-down text-[10px]"></i> DOWNLOAD RESUME
                </a>
            </div>
        </div>

        <!-- ══════════ BENTO GRID 1 ══════════ -->
        <div class="grid grid-cols-1 md:grid-cols-3 gap-3 mb-3">

            <!-- LEFT -->
            <div class="flex flex-col gap-3">
                <!-- Identity -->
                <div class="bento-card p-6 relative overflow-hidden group stagger-item" style="animation-delay: 0.1s;">
                    <div class="card-glow card-glow--cyan absolute top-0 right-0 w-40 h-40 -mr-12 -mt-12 rounded-full pointer-events-none"></div>
                    <div class="flex gap-4 items-center mb-5 relative z-10">
                        <div class="icon-box w-10 h-10 rounded-2xl flex items-center justify-center">
                            <i class="fa-regular fa-id-badge text-sm text-blue-400"></i>
                        </div>
                        <div>
                            <div class="font-bold text-[15px] text-white tracking-tight">Personal Identity</div>
                            <div class="text-[9px] text-cyan-400 tracking-[0.18em] uppercase font-bold mt-0.5">GAME DEVELOPER</div>
                        </div>
                    </div>
                    <div class="grid grid-cols-2 gap-y-4 gap-x-3 relative z-10">
                        {#each [
                            { label: 'Age', value: '21', unit: 'YRS' },
                            { label: 'Location', value: 'Algeria', unit: '' },
                            { label: 'Experience', value: '6+', unit: 'YRS' },
                            { label: 'Specialty', value: 'Game Dev', unit: '' },
                        ] as item}
                            <div class="flex flex-col gap-0.5 hover:text-blue-400 transition-colors">
                                <div class="text-[9px] text-white/35 font-bold uppercase tracking-[0.18em]">{item.label}</div>
                                <div class="font-bold text-[14px] text-white">
                                    {item.value}
                                    {#if item.unit}<span class="text-white/35 text-[9px] font-normal ml-0.5">{item.unit}</span>{/if}
                                </div>
                            </div>
                        {/each}
                    </div>
                </div>

                <!-- Roadmap -->
                <div class="bento-card p-6 relative overflow-hidden group flex-1 stagger-item" style="animation-delay: 0.2s;">
                    <div class="absolute bottom-0 left-0 w-full h-[3px] bg-white/5">
                        <div class="h-full bg-blue-400 w-[75%] shadow-[0_0_12px_rgba(59,130,246,0.7)] relative">
                            <div class="absolute right-0 top-1/2 -translate-y-1/2 w-2 h-2 bg-white rounded-full shadow-[0_0_5px_#fff]"></div>
                        </div>
                    </div>
                    <div class="flex justify-between items-center mb-5 relative z-10">
                        <div class="font-bold text-[15px] text-white tracking-tight">Game Dev Roadmap</div>
                        <div class="text-[9px] bg-blue-500/10 text-blue-400 border border-blue-500/20 px-3 py-1 rounded-full font-bold shadow-[0_0_10px_rgba(59,130,246,0.08)]">75%</div>
                    </div>
                    <div class="space-y-3 relative z-10">
                        {#each [
                            { done: true, label: 'Game Engine Mastery' },
                            { done: true, label: 'Jannah Brand Identity' },
                            { done: true, label: 'Multiplayer Systems' },
                            { done: false, label: 'VR/AR Integration' },
                        ] as item}
                            <div class="flex items-start gap-3 {item.done ? '' : 'opacity-40 hover:opacity-100 transition-opacity'}">
                                {#if item.done}
                                    <i class="fa-solid fa-check text-blue-400 text-[11px] mt-0.5 drop-shadow-[0_0_5px_rgba(59,130,246,0.7)]"></i>
                                {:else}
                                    <div class="mt-1 w-3 h-3 rounded-full border-2 border-white/30 shrink-0"></div>
                                {/if}
                                <div class="text-[12px] font-medium text-white/80">{item.label}</div>
                            </div>
                        {/each}
                    </div>
                </div>
            </div>

            <!-- MIDDLE -->
            <div class="flex flex-col gap-3">
                <!-- Activity -->
                <div class="bento-card p-6 relative overflow-hidden group flex-1 stagger-item" style="animation-delay: 0.3s;">
                    <div class="card-glow card-glow--pink absolute top-0 left-0 w-36 h-36 -ml-10 -mt-10 rounded-full pointer-events-none"></div>
                    <div class="flex justify-between items-center mb-4 relative z-10">
                        <div class="font-bold text-[15px] text-white tracking-tight">Activity Stream</div>
                        <a href="https://github.com/ch-oussama" target="_blank" rel="noopener noreferrer" class="github-badge inline-flex items-center gap-1.5 px-3 py-1.5 rounded-full text-[9px] font-bold tracking-wider">
                            <i class="fa-brands fa-github text-[11px]"></i> GITHUB
                        </a>
                    </div>
                    <div class="grid grid-cols-[28px_1fr] gap-2 items-end mb-3 relative z-10">
                        <div class="flex flex-col justify-between h-[84px] text-[9px] text-white/35 pb-1 font-mono">
                            <span>Mon</span><span>Wed</span><span>Fri</span>
                        </div>
                        <div class="flex gap-1 h-[84px] overflow-hidden">
                            {#each activityGrid as col}
                                <div class="flex flex-col gap-1 flex-1">
                                    {#each col as opacity}
                                        <div class="rounded-[2px] flex-1 hover:scale-125 transition-transform cursor-default" style="background-color: #3b82f6; opacity: {opacity}; box-shadow: {opacity > 0.7 ? '0 0 4px rgba(59,130,246,0.6)' : 'none'};"></div>
                                    {/each}
                                </div>
                            {/each}
                        </div>
                    </div>
                    <div class="flex justify-between items-center text-[9px] text-white/30 relative z-10 font-mono uppercase tracking-[0.15em]">
                        <span>Contribution Graph</span>
                        <div class="flex items-center gap-1.5">
                            Less
                            <div class="flex gap-1 mx-1">
                                {#each [0.08, 0.3, 0.6, 1] as op}
                                    <div class="w-2.5 h-2.5 rounded-[2px]" style="background-color: #3b82f6; opacity: {op};"></div>
                                {/each}
                            </div>
                            More
                        </div>
                    </div>
                </div>

                <!-- Current Focus -->
                <div class="bento-card p-6 relative overflow-hidden group stagger-item" style="animation-delay: 0.4s;">
                    <div class="font-bold text-[15px] mb-4 text-white tracking-tight">Current Focus</div>
                    <div class="relative z-10">
                        <div class="flex justify-between items-center mb-3">
                            <div class="focus-tag text-[9px] px-3 py-1 rounded-full font-bold tracking-[0.18em] uppercase">GAME DEV</div>
                            <div class="text-[11px] text-white/50 font-mono font-bold">85%</div>
                        </div>
                        <div class="font-bold text-[13px] mb-4 text-white leading-snug">Jannah: Game Development & Interactive Experiences</div>
                        <div class="w-full h-1.5 bg-white/5 rounded-full overflow-hidden">
                            <div class="h-full rounded-full progress-gradient w-[85%] relative">
                                <div class="absolute right-0 top-0 bottom-0 w-5 bg-white/30 blur-sm"></div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>

            <!-- RIGHT - Terminal -->
            <div class="flex flex-col h-full min-h-[380px] stagger-item" style="animation-delay: 0.5s;">
                <div class="bento-card flex-1 flex flex-col relative overflow-hidden group p-0 terminal-card">
                    <div class="terminal-header flex justify-between items-center px-5 py-3 border-b border-white/[0.06] relative z-10">
                        <div class="flex items-center gap-2.5 font-mono text-[10px] text-white/45 tracking-[0.25em] uppercase">
                            <i class="fa-solid fa-microchip text-blue-400/60 text-[13px]"></i>
                            GAME_CORE
                        </div>
                        <div class="flex gap-2">
                            {#each ['#ef4444', '#f59e0b', '#22c55e'] as color}
                                <button role="button" aria-label="window control" class="w-3 h-3 rounded-full border transition-all duration-200 cursor-pointer bg-transparent hover:scale-125" style="border-color: rgba(255,255,255,0.15);" onmouseenter={e => (e.currentTarget as HTMLElement).style.background = color} onmouseleave={e => (e.currentTarget as HTMLElement).style.background = 'transparent'}></button>
                            {/each}
                        </div>
                    </div>
                    <div class="flex-1 p-4 font-mono text-[10px] space-y-2 text-white/45 relative z-10 overflow-y-auto custom-scrollbar">
                        {#each logLines as line}
                            <div class="flex gap-3 items-start log-line rounded-lg px-2 py-1 -mx-2 cursor-default transition-colors">
                                <span class="text-white/25 shrink-0 tabular-nums">{line.time}</span>
                                <div class="leading-relaxed">
                                    <span class="font-bold" style="color: {line.levelColor};">{line.level}</span>
                                    {' '}{line.msg}
                                </div>
                            </div>
                        {/each}
                        <div class="flex items-center gap-2 pt-1 px-2 -mx-2">
                            <span class="text-blue-400 text-[11px]">▶</span>
                            <span class="w-[6px] h-[12px] bg-white/65 rounded-[1px] transition-opacity duration-300" style="opacity: {cursorVisible ? 1 : 0}"></span>
                        </div>
                    </div>
                    <div class="grid grid-cols-3 border-t border-white/[0.06] terminal-footer text-[9px] relative z-10">
                        <div class="stat-cell p-3 border-r border-white/[0.06] flex flex-col gap-1 items-center justify-center">
                            <span class="text-white/35 tracking-[0.2em] font-mono uppercase">MEM</span>
                            <span class="text-white font-bold text-[11px]">{memValue}</span>
                        </div>
                        <div class="stat-cell p-3 border-r border-white/[0.06] flex flex-col gap-1 items-center justify-center">
                            <span class="text-white/35 tracking-[0.2em] font-mono uppercase">CPU</span>
                            <span class="font-bold text-[11px] text-cyan-400 drop-shadow-[0_0_6px_rgba(56,189,248,0.5)]">{cpuValue}</span>
                        </div>
                        <div class="stat-cell p-3 flex flex-col gap-1 items-center justify-center">
                            <span class="text-white/35 tracking-[0.2em] font-mono uppercase">NET</span>
                            <span class="font-bold text-[11px] text-blue-400 drop-shadow-[0_0_6px_rgba(59,130,246,0.5)]">{netValue}</span>
                        </div>
                    </div>
                </div>
            </div>
        </div>

        <!-- ══════════ CODE SKILLS ══════════ -->
        <div class="bento-card p-6 mb-3 stagger-item" style="animation-delay: 0.55s;">
            <div class="flex gap-4 items-center mb-5">
                <div class="icon-box w-10 h-10 rounded-2xl flex items-center justify-center">
                    <i class="fa-solid fa-terminal text-sm text-blue-400"></i>
                </div>
                <div>
                    <div class="font-bold text-[15px] text-white tracking-tight">Code Proficiency</div>
                    <div class="text-[9px] text-cyan-400 tracking-[0.18em] uppercase font-bold mt-0.5">DAILY GRIND</div>
                </div>
            </div>
            <div class="grid grid-cols-1 sm:grid-cols-2 gap-x-8 gap-y-3">
                {#each codeSkills as skill}
                    <div class="flex flex-col gap-1.5">
                        <div class="flex justify-between items-center">
                            <span class="text-[12px] text-white/70 font-medium">{skill.name}</span>
                            <span class="text-[10px] text-blue-400 font-mono font-bold">{skill.level}%</span>
                        </div>
                        <div class="w-full h-1.5 bg-white/5 rounded-full overflow-hidden">
                            <div class="h-full rounded-full bg-gradient-to-r from-blue-500 to-cyan-400 transition-all duration-1000" style="width: {skill.level}%; box-shadow: 0 0 8px rgba(59,130,246,0.4);"></div>
                        </div>
                    </div>
                {/each}
            </div>
        </div>

        <!-- ══════════ TOOLS ══════════ -->
        <div class="bento-card p-6 mb-3 stagger-item" style="animation-delay: 0.6s;">
            <div class="flex gap-4 items-center mb-5">
                <div class="icon-box w-10 h-10 rounded-2xl flex items-center justify-center">
                    <i class="fa-solid fa-wrench text-sm text-cyan-400"></i>
                </div>
                <div>
                    <div class="font-bold text-[15px] text-white tracking-tight">Tools &amp; Ecosystem</div>
                    <div class="text-[9px] text-cyan-400 tracking-[0.18em] uppercase font-bold mt-0.5">ARSENAL</div>
                </div>
            </div>
            <div class="flex flex-wrap gap-2">
                {#each tools as tool}
                    <span class="text-[11px] font-medium px-3 py-1.5 rounded-full bg-white/5 border border-white/10 text-white/60 hover:bg-blue-500/10 hover:border-blue-500/20 hover:text-blue-400 transition-all duration-300 cursor-default">{tool}</span>
                {/each}
            </div>
        </div>

        <!-- ══════════ SERVICES HEADER ══════════ -->
        <div class="mb-3 stagger-item" style="animation-delay: 0.65s;">
            <div class="text-[10px] text-blue-400 font-bold tracking-[3px] mb-2 uppercase font-mono drop-shadow-[0_0_5px_rgba(59,130,246,0.3)]">[ WHAT_I_BUILD ]</div>
            <h2 class="font-heading text-3xl lg:text-4xl uppercase tracking-tighter text-white">GAME DEVELOPMENT<br><span class="text-white/30">&amp; ENGINEERING SOLUTIONS</span></h2>
        </div>

        <!-- ══════════ SERVICES GRID ══════════ -->
        <div class="grid grid-cols-1 sm:grid-cols-2 gap-3 mb-3">
            {#each services as service, i}
                <div class="bento-card p-6 relative overflow-hidden group stagger-item border-glow hover-glow cursor-default" style="animation-delay: {0.7 + i * 0.05}s;">
                    <div class="flex gap-4 items-start mb-4">
                        <div class="icon-box w-10 h-10 rounded-2xl flex items-center justify-center shrink-0 group-hover:scale-110 transition-transform duration-300">
                            <i class="{service.icon} text-sm text-blue-400"></i>
                        </div>
                        <div>
                            <div class="font-bold text-[14px] text-white tracking-tight mb-1">{service.title}</div>
                            <p class="text-[11px] text-white/40 leading-relaxed">{service.desc}</p>
                        </div>
                    </div>
                    <div class="flex flex-wrap gap-1.5 ml-14">
                        {#each service.tags as tag}
                            <span class="text-[9px] font-bold px-2 py-1 rounded-full bg-blue-500/10 border border-blue-500/20 text-blue-400/70">{tag}</span>
                        {/each}
                    </div>
                </div>
            {/each}
        </div>

        <!-- ══════════ TECH STACK ══════════ -->
        <div class="mb-3 stagger-item" style="animation-delay: 0.9s;">
            <div class="text-[10px] text-blue-400 font-bold tracking-[3px] mb-2 uppercase font-mono drop-shadow-[0_0_5px_rgba(59,130,246,0.3)]">[ TECHNICAL_ARSNL ]</div>
            <h2 class="font-heading text-3xl lg:text-4xl uppercase tracking-tighter text-white">ENGINES &amp; <span class="text-gradient">TECHNOLOGIES</span></h2>
        </div>

        <div class="grid grid-cols-1 md:grid-cols-3 gap-3 mb-3">
            {#each techStack as section, si}
                <div class="bento-card p-6 stagger-item" style="animation-delay: {0.95 + si * 0.05}s;">
                    <div class="font-bold text-[12px] text-white/50 tracking-[0.2em] uppercase mb-5 font-mono">{section.category}</div>
                    <div class="space-y-4">
                        {#each section.items as item}
                            <div class="flex flex-col gap-1.5">
                                <div class="flex justify-between items-center">
                                    <span class="text-[12px] text-white/70 font-medium">{item.name}</span>
                                    <span class="text-[10px] text-blue-400 font-mono font-bold">{item.pct}%</span>
                                </div>
                                <div class="w-full h-1.5 bg-white/5 rounded-full overflow-hidden">
                                    <div class="h-full rounded-full bg-gradient-to-r from-blue-500 to-cyan-400" style="width: {item.pct}%; box-shadow: 0 0 6px rgba(59,130,246,0.3);"></div>
                                </div>
                            </div>
                        {/each}
                    </div>
                </div>
            {/each}
        </div>

    </div>
</div>

<style>
    .hero-card {
        background: linear-gradient(135deg, rgba(255,255,255,0.04) 0%, rgba(255,255,255,0.01) 100%);
        border: 1px solid rgba(255,255,255,0.08);
        border-radius: 24px;
        backdrop-filter: blur(20px);
        box-shadow: 0 8px 40px rgba(0,0,0,0.35);
        transition: border-color 0.4s, box-shadow 0.4s, transform 0.3s;
    }
    .hero-card:hover {
        border-color: rgba(59, 130, 246, 0.2);
        box-shadow: 0 8px 50px rgba(59, 130, 246, 0.1);
        transform: translateY(-2px);
    }
    .watermark-text {
        color: rgba(255,255,255,0.03);
        transition: color 0.7s;
        line-height: 1;
    }
    .hero-card:hover .watermark-text { color: rgba(255,255,255,0.05); }
    .scan-overlay {
        background: repeating-linear-gradient(0deg, transparent, transparent 2px, rgba(255,255,255,0.01) 2px, rgba(255,255,255,0.01) 3px);
    }
    .avatar-glow {
        background: radial-gradient(circle, rgba(59, 130, 246, 0.4) 0%, transparent 70%);
        filter: blur(20px);
        transform: scale(1.1);
        transition: opacity 0.4s;
    }
    .hero-card:hover .avatar-glow { opacity: 0.7; }
    .role-badge {
        background: rgba(59, 130, 246, 0.08);
        border: 1px solid rgba(59, 130, 246, 0.18);
        color: #60a5fa;
    }
    .status-badge {
        background: rgba(59, 130, 246, 0.08);
        border: 1px solid rgba(59, 130, 246, 0.22);
        color: #60a5fa;
    }
    .dl-btn {
        background: rgba(255,255,255,0.05);
        border: 1px solid rgba(255,255,255,0.1);
        color: rgba(255,255,255,0.85);
    }
    .dl-btn:hover {
        background: rgba(59, 130, 246, 0.1);
        border-color: rgba(59, 130, 246, 0.3);
        color: #60a5fa;
    }
    .bento-card {
        background: linear-gradient(145deg, rgba(255,255,255,0.04) 0%, rgba(255,255,255,0.01) 100%);
        border: 1px solid rgba(255,255,255,0.07);
        border-radius: 20px;
        backdrop-filter: blur(20px);
        box-shadow: 0 4px 30px rgba(0,0,0,0.28);
        transition: border-color 0.35s, box-shadow 0.35s, transform 0.25s;
    }
    .bento-card:hover {
        border-color: rgba(59, 130, 246, 0.2);
        box-shadow: 0 8px 35px rgba(59, 130, 246, 0.08);
        transform: translateY(-2px);
    }
    .card-glow { opacity: 0; transition: opacity 0.4s; }
    .bento-card:hover .card-glow { opacity: 1; }
    .card-glow--cyan { background: radial-gradient(circle, rgba(56, 189, 248, 0.12) 0%, transparent 70%); }
    .card-glow--pink { background: radial-gradient(circle, rgba(59, 130, 246, 0.12) 0%, transparent 70%); }
    .icon-box {
        background: linear-gradient(135deg, rgba(59, 130, 246, 0.1), rgba(59, 130, 246, 0.03));
        border: 1px solid rgba(59, 130, 246, 0.15);
    }
    .github-badge {
        background: rgba(59, 130, 246, 0.08);
        border: 1px solid rgba(59, 130, 246, 0.2);
        color: #60a5fa;
        transition: background 0.2s;
    }
    .github-badge:hover { background: rgba(59, 130, 246, 0.15); }
    .focus-tag {
        background: rgba(59, 130, 246, 0.12);
        border: 1px solid rgba(59, 130, 246, 0.2);
        color: #60a5fa;
    }
    .progress-gradient {
        background: linear-gradient(90deg, #3b82f6, #60a5fa, #38bdf8);
        box-shadow: 0 0 12px rgba(59, 130, 246, 0.5);
    }
    .terminal-card {
        background: linear-gradient(160deg, rgba(5,5,10,0.85) 0%, rgba(10,8,20,0.9) 100%);
        border-color: rgba(255,255,255,0.07);
    }
    .terminal-header {
        background: rgba(255,255,255,0.015);
        backdrop-filter: blur(10px);
    }
    .terminal-footer {
        background: rgba(0,0,0,0.35);
        backdrop-filter: blur(10px);
    }
    .log-line:hover { background: rgba(255,255,255,0.04); }
    .stat-cell { transition: background 0.2s; }
    .stat-cell:hover { background: rgba(255,255,255,0.04); }
</style>
