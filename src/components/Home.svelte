<script lang="ts">
    import { onMount } from 'svelte';
    
    let particlesContainer: HTMLDivElement;
    let mouseX = 0;
    let mouseY = 0;
    let cursorX = 0;
    let cursorY = 0;
    let typedText = '';
    const fullText = 'SENIOR ENGINEER';
    let charIndex = 0;
    let isHovering = false;
    
    // Card tilt state
    let cardTilt = { x: 0, y: 0 };
    
    onMount(() => {
        // Particle system
        const particleCount = 50;
        for (let i = 0; i < particleCount; i++) {
            const size = Math.random() * 3 + 1;
            const x = Math.random() * 100;
            const y = Math.random() * 100;
            const opacity = Math.random() * 0.4 + 0.1;
            const duration = Math.random() * 20 + 10;
            
            const p = document.createElement('div');
            const colors = [
                '59, 130, 246',
                '96, 165, 250',
                '56, 189, 248',
                '129, 140, 248',
                '147, 197, 253',
            ];
            const color = colors[Math.floor(Math.random() * colors.length)];
            p.style.cssText = `
                position: absolute;
                width: ${size}px;
                height: ${size}px;
                background-color: rgba(${color}, ${opacity});
                box-shadow: 0 0 10px rgba(${color}, ${opacity + 0.2});
                border-radius: 50%;
                left: ${x}%;
                top: ${y}%;
                animation: float-home ${duration}s infinite linear alternate;
            `;
            particlesContainer.appendChild(p);
        }
        
        if (!document.getElementById('home-keyframes')) {
            const style = document.createElement('style');
            style.id = 'home-keyframes';
            style.innerHTML = `
            @keyframes float-home {
                0% { transform: translate(0, 0) scale(1); }
                50% { transform: translate(${Math.random() * 60 - 30}px, ${Math.random() * -60 - 20}px) scale(1.1); }
                100% { transform: translate(${Math.random() * 40 - 20}px, ${Math.random() * -40 - 10}px) scale(0.9); }
            }
            `;
            document.head.appendChild(style);
        }
        
        // Typing animation
        const typeInterval = setInterval(() => {
            if (charIndex < fullText.length) {
                typedText += fullText[charIndex];
                charIndex++;
            } else {
                clearInterval(typeInterval);
            }
        }, 100);
        
        // Smooth mouse tracking
        const handleMouseMove = (e: MouseEvent) => {
            mouseX = e.clientX;
            mouseY = e.clientY;
        };
        
        let animFrame: number;
        const animate = () => {
            cursorX += (mouseX - cursorX) * 0.15;
            cursorY += (mouseY - cursorY) * 0.15;
            animFrame = requestAnimationFrame(animate);
        };
        
        window.addEventListener('mousemove', handleMouseMove);
        animFrame = requestAnimationFrame(animate);
        
        return () => {
            window.removeEventListener('mousemove', handleMouseMove);
            cancelAnimationFrame(animFrame);
            clearInterval(typeInterval);
        };
    });
    
    function handleCardMouseMove(e: MouseEvent) {
        const rect = (e.currentTarget as HTMLElement).getBoundingClientRect();
        const x = e.clientX - rect.left;
        const y = e.clientY - rect.top;
        const centerX = rect.width / 2;
        const centerY = rect.height / 2;
        cardTilt = {
            x: (y - centerY) / 20,
            y: (centerX - x) / 20
        };
    }
    
    function handleCardMouseLeave() {
        cardTilt = { x: 0, y: 0 };
    }
</script>

<div class="absolute inset-0 -z-10 overflow-hidden" bind:this={particlesContainer}></div>

<!-- Tiny mouse follower -->
<div 
    class="fixed pointer-events-none z-[9999] transition-transform duration-100 ease-out"
    style="left: {cursorX}px; top: {cursorY}px; transform: translate(-50%, -50%) scale({isHovering ? 1.5 : 1});"
>
    <div class="relative">
        <div class="absolute inset-[-8px] rounded-full border border-blue-400/30 transition-all duration-300 {isHovering ? 'border-blue-400/60 scale-125' : ''}"></div>
        <div class="w-[40px] h-[40px] rounded-full overflow-hidden border-2 border-white/30 shadow-[0_0_15px_rgba(59,130,246,0.5)] transition-all duration-300">
            <img src="/loge.png" alt="" class="w-full h-full object-cover">
        </div>
    </div>
</div>

<div class="flex flex-col lg:flex-row justify-between items-center px-[5%] h-full pt-20 pb-10">
    <!-- Left -->
    <div class="flex-1 max-w-[500px] w-full text-center lg:text-left z-10">
        <div class="inline-block text-[10px] font-bold tracking-[2px] mb-6 font-mono uppercase border border-white/10 bg-white/5 px-3 py-1 rounded-full text-blue-400 shadow-[0_0_15px_rgba(59,130,246,0.15)] animate-bounce-in smooth-btn hover:border-blue-400/30 hover:shadow-[0_0_25px_rgba(59,130,246,0.3)] cursor-pointer"
            on:mouseenter={() => isHovering = true}
            on:mouseleave={() => isHovering = false}
        >
            <span class="animate-pulse inline-block w-1.5 h-1.5 bg-blue-400 rounded-full mr-2"></span>
            DISCORD_UID: 340489264599007255
        </div>
        
        <h1 class="font-heading text-[10vw] lg:text-[5vw] leading-[1.05] mb-6 uppercase tracking-[-2px] text-white drop-shadow-2xl">
            HI, I'M OUSSAMA<br>
            <span class="text-[7.5vw] lg:text-[4vw] text-gradient text-scramble">{typedText}<span class="typing-cursor"></span></span><br>
            <span class="text-white/60">ENGINEER</span>
        </h1>
        
        <div class="text-[13px] font-medium text-white/50 bg-white/5 border border-white/10 px-5 py-3 rounded-xl inline-block mb-10 text-left backdrop-blur-md shadow-[0_8px_32px_0_rgba(0,0,0,0.3)] hover-glow border-glow cursor-default"
            on:mouseenter={() => isHovering = true}
            on:mouseleave={() => isHovering = false}
        >
            Founder of <span class="text-blue-400 font-bold">Jannah</span>, probably building something <span class="text-blue-400 font-bold">extraordinary</span> right now.
        </div>
        
        <div class="flex gap-4 justify-center lg:justify-start">
            <div class="glass-card px-4 py-3 flex items-center gap-3 smooth-btn hover-glow border-glow cursor-pointer"
                on:mouseenter={() => isHovering = true}
                on:mouseleave={() => isHovering = false}
            >
                <div class="img-zoom rounded-full">
                    <img src="https://flagcdn.com/w40/dz.png" alt="Algeria" class="w-[35px] h-[35px] rounded-full object-cover border border-white/20 shadow-md">
                </div>
                <div class="text-left">
                    <strong class="block text-[12px] leading-tight text-white">_algerian</strong>
                    <span class="text-[10px] text-white/40">Based in Algeria</span>
                </div>
            </div>
            <a href="https://jawhar-bay.vercel.app/" target="_blank" rel="noopener noreferrer" 
                class="glass-card px-4 py-3 flex items-center gap-3 group cursor-pointer smooth-btn border-glow ripple"
                on:mouseenter={() => isHovering = true}
                on:mouseleave={() => isHovering = false}
            >
                <div class="w-[35px] h-[35px] bg-gradient-to-br from-blue-400 to-blue-600 text-white rounded-lg flex justify-center items-center shadow-[0_0_15px_rgba(59,130,246,0.4)] group-hover:scale-110 group-hover:shadow-[0_0_25px_rgba(59,130,246,0.6)] transition-all duration-300">
                    <i class="fa-solid fa-gem"></i>
                </div>
                <div class="text-left">
                    <strong class="block text-[12px] leading-tight text-white">جنة</strong>
                    <span class="text-[10px] text-white/40">+50 Projects</span>
                </div>
                <div class="bg-blue-500/20 text-blue-400 border border-blue-500/30 text-[10px] px-2 py-0.5 rounded-full font-bold ml-2 shadow-[0_0_10px_rgba(59,130,246,0.2)] glow-pulse">
                    Pro
                </div>
            </a>
        </div>
    </div>
    

    <!-- Center - Avatar -->
    <div class="hidden lg:flex flex-1 justify-center items-center z-10 -mx-10">
        <div class="relative group cursor-pointer"
            on:mouseenter={() => isHovering = true}
            on:mouseleave={() => isHovering = false}
        >
            <div class="absolute inset-[-30px] rounded-full bg-gradient-to-br from-blue-500/20 via-cyan-500/10 to-indigo-500/20 blur-2xl animate-pulse-slow group-hover:from-blue-500/30 group-hover:via-cyan-500/20 group-hover:to-indigo-500/30 transition-all duration-500"></div>
            <div class="absolute inset-[-18px] rounded-full border-2 border-dashed border-blue-400/30 animate-rotate-slow group-hover:border-blue-400/50 transition-colors duration-300"></div>
            <div class="relative w-[220px] h-[220px] rounded-full overflow-hidden border-2 border-white/20 shadow-[0_0_40px_rgba(59,130,246,0.3)] group-hover:shadow-[0_0_80px_rgba(59,130,246,0.6)] transition-all duration-500 group-hover:scale-105">
                <img src="/loge.png" alt="Oussama" class="w-full h-full object-cover group-hover:scale-110 transition-transform duration-700">
                <div class="absolute inset-0 bg-gradient-to-t from-[#0a0a0f]/30 to-transparent"></div>
            </div>
            <div class="absolute bottom-5 right-5 w-4 h-4 bg-blue-400 rounded-full border-2 border-[#0a0a0f] shadow-[0_0_10px_rgba(59,130,246,0.8)] animate-pulse group-hover:scale-125 transition-transform duration-300"></div>
            <div class="absolute -bottom-10 left-1/2 -translate-x-1/2 bg-white/10 backdrop-blur-md px-4 py-1.5 rounded-full border border-white/10 whitespace-nowrap group-hover:bg-white/15 group-hover:border-blue-400/30 transition-all duration-300">
                <span class="text-[10px] font-bold text-blue-400 tracking-wider uppercase">Senior Engineer</span>
            </div>
        </div>
    </div>

    <!-- Right -->
    <div class="flex-1 max-w-[350px] w-full flex flex-col gap-[50px] text-center lg:text-right hidden md:flex z-10">
        <div class="flex gap-[30px] justify-center lg:justify-end">
            <div class="glass-card p-5 w-[160px] stagger-item border-glow hover-glow cursor-default"
                style="animation-delay: 0.1s; transform: perspective(1000px) rotateX({cardTilt.x}deg) rotateY({cardTilt.y}deg);"
                on:mousemove={handleCardMouseMove}
                on:mouseleave={handleCardMouseLeave}
                on:mouseenter={() => isHovering = true}
            >
                <h4 class="text-[10px] text-blue-400 mb-[15px] tracking-[2px] font-mono flex items-center justify-center lg:justify-end"><i class="fa-solid fa-layer-group mr-2"></i>EXPERTISE</h4>
                <ul class="text-[12px] text-white/80 font-medium space-y-3">
                    <li class="flex items-center lg:justify-end gap-2 hover:text-blue-400 transition-all duration-300 cursor-default hover:translate-x-[-5px]"><span class="w-1.5 h-1.5 bg-blue-400/50 rounded-full group-hover:bg-blue-400 transition-colors"></span> Game Development</li>
                    <li class="flex items-center lg:justify-end gap-2 hover:text-blue-400 transition-all duration-300 cursor-default hover:translate-x-[-5px]"><span class="w-1.5 h-1.5 bg-blue-400/50 rounded-full"></span> Game Design</li>
                    <li class="flex items-center lg:justify-end gap-2 hover:text-blue-400 transition-all duration-300 cursor-default hover:translate-x-[-5px]"><span class="w-1.5 h-1.5 bg-blue-400/50 rounded-full"></span> Multiplayer Systems</li>
                </ul>
            </div>
            <div class="glass-card p-5 w-[160px] stagger-item border-glow hover-glow cursor-default"
                style="animation-delay: 0.2s; transform: perspective(1000px) rotateX({cardTilt.x}deg) rotateY({cardTilt.y}deg);"
                on:mousemove={handleCardMouseMove}
                on:mouseleave={handleCardMouseLeave}
                on:mouseenter={() => isHovering = true}
            >
                <h4 class="text-[10px] text-cyan-400 mb-[15px] tracking-[2px] font-mono flex items-center justify-center lg:justify-end"><i class="fa-solid fa-box-open mr-2"></i>FOCUS</h4>
                <ul class="text-[12px] text-white/80 font-medium space-y-3">
                    <li class="flex items-center lg:justify-end gap-2 hover:text-cyan-400 transition-all duration-300 cursor-default hover:translate-x-[-5px]"><span class="w-1.5 h-1.5 bg-cyan-400/50 rounded-full"></span> Unity / Unreal</li>
                    <li class="flex items-center lg:justify-end gap-2 hover:text-cyan-400 transition-all duration-300 cursor-default hover:translate-x-[-5px]"><span class="w-1.5 h-1.5 bg-cyan-400/50 rounded-full"></span> 3D / 2D Graphics</li>
                    <li class="flex items-center lg:justify-end gap-2 hover:text-cyan-400 transition-all duration-300 cursor-default hover:translate-x-[-5px]"><span class="w-1.5 h-1.5 bg-cyan-400/50 rounded-full"></span> AI / Physics</li>
                </ul>
            </div>
        </div>
        
        <div class="stagger-item" style="animation-delay: 0.3s;">
            <div class="text-[10px] text-white/30 tracking-[3px] mb-[20px] font-mono text-scramble">[ CONNECT_NODES ]</div>
            <div class="flex justify-center lg:justify-end gap-[12px] mb-[20px]">
                <a href="https://github.com/ch-oussama" target="_blank" rel="noopener noreferrer" class="w-[40px] h-[40px] glass-card flex justify-center items-center text-[18px] text-white/40 hover:text-blue-400 hover:border-blue-500/30 hover:shadow-[0_0_20px_rgba(59,130,246,0.4)] transition-all duration-300 magnetic-btn ripple icon-hover"
                    on:mouseenter={() => isHovering = true}
                    on:mouseleave={() => isHovering = false}
                ><i class="fa-brands fa-github"></i></a>
                <a href="https://www.instagram.com/oussama_dev.rt/" target="_blank" rel="noopener noreferrer" class="w-[40px] h-[40px] glass-card flex justify-center items-center text-[18px] text-white/40 hover:text-pink-400 hover:border-pink-500/30 hover:shadow-[0_0_20px_rgba(236,72,153,0.4)] transition-all duration-300 magnetic-btn ripple icon-hover"
                    on:mouseenter={() => isHovering = true}
                    on:mouseleave={() => isHovering = false}
                ><i class="fa-brands fa-instagram"></i></a>
                <a href="https://www.instagram.com/oussama_dev.rt/" target="_blank" rel="noopener noreferrer" class="w-[40px] h-[40px] glass-card flex justify-center items-center text-[18px] text-white/40 hover:text-blue-400 hover:border-blue-400/30 hover:shadow-[0_0_20px_rgba(59,130,246,0.4)] transition-all duration-300 magnetic-btn ripple icon-hover"
                    on:mouseenter={() => isHovering = true}
                    on:mouseleave={() => isHovering = false}
                ><i class="fa-brands fa-behance"></i></a>
                <a href="https://discord.gg/DV3FcBrtM9" target="_blank" rel="noopener noreferrer" class="w-[40px] h-[40px] glass-card flex justify-center items-center text-[18px] text-white/40 hover:text-indigo-400 hover:border-indigo-500/30 hover:shadow-[0_0_20px_rgba(129,140,248,0.4)] transition-all duration-300 magnetic-btn ripple icon-hover"
                    on:mouseenter={() => isHovering = true}
                    on:mouseleave={() => isHovering = false}
                ><i class="fa-brands fa-discord"></i></a>
            </div>
            <p class="text-[11px] text-white/35 leading-[1.8] max-w-[260px] mx-auto lg:ml-auto lg:mr-0 glass-card p-4 hover:border-white/20 transition-all hover-glow border-glow cursor-default"
                on:mouseenter={() => isHovering = true}
                on:mouseleave={() => isHovering = false}
            >
                Engineering game experiences with a focus on immersive gameplay, stunning visuals, and cutting-edge game technology.
            </p>
        </div>
    </div>
</div>
