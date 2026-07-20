<script lang="ts">
    import { createEventDispatcher } from 'svelte';
    
    export let currentSection: string;
    
    const dispatch = createEventDispatcher();
    
    function navigate(section: string) {
        dispatch('navigate', section);
    }
</script>

<div class="fixed top-6 left-1/2 transform -translate-x-1/2 z-50 w-[90%] max-w-[800px]">
    <nav class="flex justify-between items-center px-6 py-3 bg-[#0a0a0f]/80 backdrop-blur-xl border border-white/10 rounded-full shadow-[0_8px_32px_0_rgba(0,0,0,0.5)] border-glow">
        <div class="flex items-center gap-2.5 cursor-pointer smooth-btn group" on:click={() => navigate('home')}>
            <div class="relative">
                <div class="absolute inset-0 rounded-full bg-blue-500 blur-[8px] opacity-40 group-hover:opacity-60 transition-opacity duration-300 animate-glow"></div>
                <img src="/loge.png" alt="Oussama" class="relative w-8 h-8 rounded-full object-cover border border-white/20 shadow-lg group-hover:border-blue-400/50 transition-all duration-300">
            </div>
            <div class="flex flex-col leading-none">
                <span class="font-heading font-bold text-[13px] tracking-[2px] text-white group-hover:text-blue-400 transition-colors duration-300">OUSSAMA</span>
                <span class="text-[9px] text-white/40 tracking-[1px] font-mono">Game Developer</span>
            </div>
        </div>
        
        <ul class="flex gap-[20px] md:gap-[40px]">
            <li>
                <button 
                    class="nav-link text-[10px] md:text-[11px] font-bold tracking-[2px] uppercase smooth-btn {currentSection === 'home' ? 'active text-blue-400' : 'text-white/40 hover:text-white'}"
                    on:click={() => navigate('home')}
                >HOME</button>
            </li>
            <li>
                <button 
                    class="nav-link text-[10px] md:text-[11px] font-bold tracking-[2px] uppercase smooth-btn {currentSection === 'about' ? 'active text-blue-400' : 'text-white/40 hover:text-white'}"
                    on:click={() => navigate('about')}
                >ABOUT</button>
            </li>
            <li>
                <button 
                    class="nav-link text-[10px] md:text-[11px] font-bold tracking-[2px] uppercase smooth-btn {currentSection === 'projects' ? 'active text-blue-400' : 'text-white/40 hover:text-white'}"
                    on:click={() => navigate('projects')}
                >PROJECTS</button>
            </li>
        </ul>
        
        <div>
            <button class="bg-blue-500/10 hover:bg-blue-500/25 text-blue-400 border border-blue-500/30 hover:border-blue-400/60 px-5 py-2 rounded-full text-[10px] font-bold tracking-[1px] transition-all duration-300 hover:shadow-[0_0_25px_rgba(59,130,246,0.4)] ripple magnetic-btn smooth-btn">
                JANNAH
            </button>
        </div>
    </nav>
</div>
