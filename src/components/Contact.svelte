<script lang="ts">
    import ConstellationField from './ConstellationField.svelte';
    import { onMount } from 'svelte';

    let name = '';
    let email = '';
    let message = '';
    let submitted = false;

    function handleSubmit(e: Event) {
        e.preventDefault();
        submitted = true;
        // Mock sending action
        setTimeout(() => {
            name = '';
            email = '';
            message = '';
            submitted = false;
            alert("Message sent successfully!");
        }, 1500);
    }
</script>

<div class="h-full w-full overflow-y-auto px-[5%] py-[20px] custom-scrollbar z-10 relative mt-20">
    <div class="pointer-events-none fixed inset-0 z-0 overflow-hidden">
        <ConstellationField />
    </div>

    <div class="relative z-10 max-w-3xl mx-auto pt-10 pb-20 animate-slide-up">
        <div class="text-[10px] text-[#E1E0CC] font-bold tracking-[3px] mb-2 uppercase font-mono drop-shadow-[0_0_5px_rgba(212,163,115,0.3)]">
            [ COMM_LINK_ESTABLISHED ]
        </div>
        <h2 class="font-heading text-4xl lg:text-5xl uppercase tracking-tighter text-white mb-8">
            SEND A<br/>MESSAGE
        </h2>

        <div class="bento-card p-8 relative overflow-hidden group">
            <div class="card-glow absolute top-0 right-0 w-64 h-64 -mr-20 -mt-20 rounded-full pointer-events-none"></div>
            
            <form on:submit={handleSubmit} class="flex flex-col gap-6 relative z-10">
                <div class="flex flex-col gap-2">
                    <label for="name" class="text-[11px] text-white/50 tracking-widest uppercase font-mono">ID / Name</label>
                    <input 
                        type="text" 
                        id="name" 
                        bind:value={name} 
                        required 
                        class="bg-white/5 border border-white/10 rounded-xl px-4 py-3 text-white placeholder-white/20 focus:outline-none focus:border-[#D4A373]/50 focus:bg-white/10 transition-all text-sm font-medium"
                        placeholder="Enter your name"
                    />
                </div>
                
                <div class="flex flex-col gap-2">
                    <label for="email" class="text-[11px] text-white/50 tracking-widest uppercase font-mono">Signal / Email</label>
                    <input 
                        type="email" 
                        id="email" 
                        bind:value={email} 
                        required 
                        class="bg-white/5 border border-white/10 rounded-xl px-4 py-3 text-white placeholder-white/20 focus:outline-none focus:border-[#D4A373]/50 focus:bg-white/10 transition-all text-sm font-medium"
                        placeholder="Enter your email address"
                    />
                </div>
                
                <div class="flex flex-col gap-2">
                    <label for="message" class="text-[11px] text-white/50 tracking-widest uppercase font-mono">Transmission</label>
                    <textarea 
                        id="message" 
                        bind:value={message} 
                        required 
                        rows="5"
                        class="bg-white/5 border border-white/10 rounded-xl px-4 py-3 text-white placeholder-white/20 focus:outline-none focus:border-[#D4A373]/50 focus:bg-white/10 transition-all text-sm font-medium resize-none custom-scrollbar"
                        placeholder="Write your message here..."
                    ></textarea>
                </div>
                
                <button 
                    type="submit" 
                    disabled={submitted}
                    class="mt-4 bg-gradient-to-r from-[#D4A373]/80 to-[#C68B4E]/80 hover:from-[#D4A373] hover:to-[#C68B4E] text-[#0E0C09] font-bold text-xs tracking-widest uppercase py-4 rounded-xl transition-all shadow-[0_0_20px_rgba(212,163,115,0.2)] hover:shadow-[0_0_30px_rgba(212,163,115,0.4)] disabled:opacity-50 disabled:cursor-not-allowed smooth-btn flex items-center justify-center gap-3"
                >
                    {#if submitted}
                        <i class="fa-solid fa-spinner fa-spin"></i> TRANSMITTING...
                    {:else}
                        <i class="fa-solid fa-paper-plane"></i> INITIATE TRANSMISSION
                    {/if}
                </button>
            </form>
        </div>
        
        <div class="mt-12 flex flex-col items-center gap-4 text-center">
            <p class="text-white/40 text-sm font-light max-w-md">Or reach out directly through other channels if you prefer a different protocol.</p>
            <div class="flex gap-4">
                <a href="https://github.com/ch-oussama" target="_blank" class="w-10 h-10 rounded-full border border-white/10 bg-white/5 flex items-center justify-center text-white/70 hover:text-white hover:bg-white/10 transition-all smooth-btn">
                    <i class="fa-brands fa-github"></i>
                </a>
                <a href="mailto:hello@example.com" class="w-10 h-10 rounded-full border border-white/10 bg-white/5 flex items-center justify-center text-white/70 hover:text-white hover:bg-white/10 transition-all smooth-btn">
                    <i class="fa-solid fa-envelope"></i>
                </a>
            </div>
        </div>
    </div>
</div>

<style>
    .bento-card {
        background: rgba(255, 255, 255, 0.02);
        border: 1px solid rgba(255, 255, 255, 0.05);
        border-radius: 24px;
        backdrop-filter: blur(16px);
        box-shadow: 0 8px 32px 0 rgba(0, 0, 0, 0.3);
    }
    .card-glow {
        background: radial-gradient(circle, rgba(212, 163, 115, 0.15) 0%, rgba(0, 0, 0, 0) 70%);
    }
    .animate-slide-up {
        animation: slideUp 0.8s cubic-bezier(0.16, 1, 0.3, 1) forwards;
    }
    @keyframes slideUp {
        0% { opacity: 0; transform: translateY(40px); }
        100% { opacity: 1; transform: translateY(0); }
    }
</style>
