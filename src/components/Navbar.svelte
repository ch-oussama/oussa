<script lang="ts">
    import { createEventDispatcher } from 'svelte';
    
    export let currentSection: string;
    
    const dispatch = createEventDispatcher();
    
    function navigate(section: string) {
        dispatch('navigate', section);
    }
</script>

<div class="fixed top-6 left-1/2 transform -translate-x-1/2 z-50 w-[90%] max-w-[800px]">
    <nav class="flex justify-between items-center px-6 py-3 bg-[#0E0C09]/80 backdrop-blur-xl border border-white/10 rounded-full shadow-[0_8px_32px_0_rgba(0,0,0,0.5)] border-glow">
        <div class="flex items-center gap-2.5 cursor-pointer smooth-btn group" on:click={() => navigate('home')}>
            <div class="relative">
                <div class="absolute inset-0 rounded-full bg-[#D4A373] blur-[8px] opacity-40 group-hover:opacity-60 transition-opacity duration-300 animate-glow"></div>
                <img src="/loge.png" alt="Oussama" class="relative w-8 h-8 rounded-full object-cover border border-white/20 shadow-lg group-hover:border-[#E1E0CC]/50 transition-all duration-300">
            </div>
            <div class="flex flex-col leading-none">
                <span class="font-heading font-bold text-[13px] tracking-[2px] text-white group-hover:text-[#E1E0CC] transition-colors duration-300">OUSSAMA</span>
                <span class="text-[9px] text-white/40 tracking-[1px] font-mono">Game Developer</span>
            </div>
        </div>
        
        <ul class="flex gap-[20px] md:gap-[40px]">
            <li>
                <button 
                    class="nav-link text-[10px] md:text-[11px] font-bold tracking-[2px] uppercase smooth-btn {currentSection === 'home' ? 'active text-[#E1E0CC]' : 'text-white/40 hover:text-white'}"
                    on:click={() => navigate('home')}
                >HOME</button>
            </li>
            <li>
                <button 
                    class="nav-link text-[10px] md:text-[11px] font-bold tracking-[2px] uppercase smooth-btn {currentSection === 'about' ? 'active text-[#E1E0CC]' : 'text-white/40 hover:text-white'}"
                    on:click={() => navigate('about')}
                >ABOUT</button>
            </li>
            <li>
                <button 
                    class="nav-link text-[10px] md:text-[11px] font-bold tracking-[2px] uppercase smooth-btn {currentSection === 'projects' ? 'active text-[#E1E0CC]' : 'text-white/40 hover:text-white'}"
                    on:click={() => navigate('projects')}
                >PROJECTS</button>
            </li>
        </ul>
        
        <div>
            <button 
                class="bg-[#D4A373]/10 hover:bg-[#D4A373]/25 text-[#E1E0CC] border border-[#D4A373]/30 hover:border-[#E1E0CC]/60 px-5 py-2 rounded-full text-[10px] font-bold tracking-[1px] transition-all duration-300 hover:shadow-[0_0_25px_rgba(212,163,115,0.4)] ripple magnetic-btn smooth-btn {currentSection === 'contact' ? 'bg-[#D4A373]/30 border-[#E1E0CC]/80 shadow-[0_0_20px_rgba(212,163,115,0.4)]' : ''}"
                on:click={() => navigate('contact')}
            >
                CONTACT
            </button>
        </div>
    </nav>
</div>
