<script lang="ts">
    import { onMount } from 'svelte';

    let canvas: HTMLCanvasElement;
    let width: number, height: number;
    let nodes: any[] = [];
    const LINK = 160; 
    let MAX_NODES = 85;
    let pointer = { x: -1000, y: -1000 };
    let frameId: number;

    function initNodes() {
        nodes = [];
        for(let i=0; i<MAX_NODES; i++) {
            nodes.push({
                x: Math.random() * width,
                y: Math.random() * height,
                vx: (Math.random() - 0.5) * 0.3,
                vy: (Math.random() - 0.5) * 0.3,
                radius: Math.random() * 2.4 + 1.8
            });
        }
    }

    function dist(a: any, b: any) {
        return Math.hypot(a.x - b.x, a.y - b.y);
    }

    onMount(() => {
        const ctx = canvas.getContext('2d');
        if (!ctx) return;
        
        const resize = () => {
            MAX_NODES = window.innerWidth < 768 ? 40 : 85;
            const dpr = Math.min(window.devicePixelRatio || 1, 2);
            width = window.innerWidth;
            height = window.innerHeight;
            canvas.width = Math.max(1, Math.floor(width * dpr));
            canvas.height = Math.max(1, Math.floor(height * dpr));
            canvas.style.width = width + 'px';
            canvas.style.height = height + 'px';
            ctx.setTransform(dpr, 0, 0, dpr, 0, 0);
            ctx.imageSmoothingEnabled = false;
        };

        window.addEventListener('resize', () => {
            resize();
            initNodes();
        });
        resize();
        initNodes();

        const handleMouseMove = (e: MouseEvent) => {
            pointer.x = e.clientX;
            pointer.y = e.clientY;
        };
        const handleMouseLeave = () => {
            pointer.x = -1000;
            pointer.y = -1000;
        };

        window.addEventListener('mousemove', handleMouseMove);
        document.addEventListener('mouseleave', handleMouseLeave);

        function animateCanvas() {
            ctx!.clearRect(0, 0, width, height);
            ctx!.lineCap = 'butt';
            ctx!.lineJoin = 'miter';
            
            ctx!.strokeStyle = '#E6C879';
            ctx!.lineWidth = 1;
            for (let i = 0; i < nodes.length; i++) {
                for (let j = i + 1; j < nodes.length; j++) {
                    const d = dist(nodes[i], nodes[j]);
                    if (d < LINK) {
                        ctx!.globalAlpha = 0.22 + (1 - d/LINK) * 0.55;
                        ctx!.beginPath();
                        ctx!.moveTo(nodes[i].x, nodes[i].y);
                        ctx!.lineTo(nodes[j].x, nodes[j].y);
                        ctx!.stroke();
                    }
                }
            }

            nodes.forEach(node => {
                node.x += node.vx;
                node.y += node.vy;
                
                if(node.x < 0 || node.x > width) node.vx *= -1;
                if(node.y < 0 || node.y > height) node.vy *= -1;

                const pd = dist(node, pointer);
                if(pd < 220) {
                    node.x -= (node.x - pointer.x) * 0.005;
                    node.y -= (node.y - pointer.y) * 0.005;
                }
                
                const pulse = 0.78 + Math.sin(Date.now() * 0.001 + node.x) * 0.22;
                ctx!.fillStyle = '#E6C879';
                ctx!.globalAlpha = pulse * 0.28;
                ctx!.beginPath();
                ctx!.arc(node.x, node.y, node.radius * 2.4, 0, Math.PI * 2);
                ctx!.fill();
                ctx!.globalAlpha = pulse;
                ctx!.beginPath();
                ctx!.arc(node.x, node.y, node.radius, 0, Math.PI * 2);
                ctx!.fill();
            });

            ctx!.globalAlpha = 1;
            frameId = requestAnimationFrame(animateCanvas);
        }
        
        frameId = requestAnimationFrame(animateCanvas);

        return () => {
            cancelAnimationFrame(frameId);
            window.removeEventListener('mousemove', handleMouseMove);
            document.removeEventListener('mouseleave', handleMouseLeave);
        };
    });
</script>

<div class="fixed inset-0 z-0 pointer-events-none bg-[radial-gradient(ellipse_at_center,_#0E1222_0%,_#070914_100%)]"></div>
<div class="fixed inset-0 z-0 pointer-events-none">
    <canvas bind:this={canvas} class="w-full h-full"></canvas>
</div>
<div class="fixed inset-0 z-0 pointer-events-none bg-gradient-to-b from-transparent via-[#0E1222]/40 to-[#070914] opacity-80"></div>
