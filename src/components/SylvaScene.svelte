<script lang="ts">
  import { onMount } from 'svelte';
  import '../shaders/threeui.css';
  import innerGreenSource from '../shaders/sylva-living-world/sources/inner-green-3d.html?raw';
  import threeRuntime from '../shaders/sylva-living-world/sources/inner-green-assets/three.min.js?raw';

  type Variant = 'living-green' | 'sakura-sunset' | 'maple-autumn' | 'sequoia-mist';
  export let variant: Variant = 'living-green';
  export let className: string = '';

  const VARIANT_LABELS: Record<Variant, string> = {
    'living-green': 'Living Green',
    'sakura-sunset': 'Sakura Sunset',
    'maple-autumn': 'Maple Autumn',
    'sequoia-mist': 'Sequoia Mist'
  };

  const VARIANT_BACKGROUNDS: Record<Variant, string> = {
    'living-green': '#4a4d44',
    'sakura-sunset': '#3c2c36',
    'maple-autumn': '#313a41',
    'sequoia-mist': '#3a3d36'
  };

  const SCENE_ONLY_MARKUP = (label: string) => `<main class="hero" id="hero">
  <canvas id="scene" role="img" aria-label="${label}"></canvas>
  <div class="stage" id="stage" aria-hidden="true"></div>
</main>`;

  const SCENE_ONLY_STYLE = `<style data-threeui-sylva-scene>
html,body{width:100%!important;height:100%!important;min-height:0!important;margin:0!important;overflow:hidden!important}
body{position:relative!important;background:#4a4d44!important}
.hero{height:100%!important;min-height:0!important}
#scene{pointer-events:auto!important}
</style>`;

  const VARIANT_STYLES: Record<Variant, string> = {
    'living-green': '',
    'sakura-sunset': `<style data-threeui-sylva-sakura-sunset>
html,body{background:#3c2c36!important}
.hero{background:radial-gradient(66% 56% at 26% 90%,rgba(255,226,212,0.125) 0%,rgba(255,226,212,0) 74%),radial-gradient(74% 64% at 92% 2%,rgba(18,10,18,0.30) 0%,rgba(18,10,18,0) 72%),#3c2c36!important}
.hero::after{background:radial-gradient(76% 48% at 44% 118%,rgba(255,218,208,0.28) 0%,rgba(255,208,200,0.095) 44%,rgba(255,202,196,0) 86%),linear-gradient(180deg,rgba(255,214,206,0) 56%,rgba(255,210,202,0.030) 78%,rgba(255,214,206,0.078) 100%)!important}
</style>`,
    'maple-autumn': `<style data-threeui-sylva-maple-autumn>
html,body{background:#313a41!important}
.hero{background:radial-gradient(66% 56% at 26% 90%,rgba(255,216,176,0.115) 0%,rgba(255,216,176,0) 74%),radial-gradient(74% 64% at 92% 2%,rgba(10,15,20,0.30) 0%,rgba(10,15,20,0) 72%),#313a41!important}
.hero::after{background:radial-gradient(76% 48% at 44% 118%,rgba(255,206,158,0.24) 0%,rgba(255,194,150,0.08) 44%,rgba(255,188,146,0) 86%),linear-gradient(180deg,rgba(255,206,160,0) 56%,rgba(255,200,156,0.028) 78%,rgba(255,206,160,0.070) 100%)!important}
</style>`,
    'sequoia-mist': `<style data-threeui-sylva-sequoia-mist>
html,body{background:#3a3d36!important}
.hero{background:radial-gradient(66% 56% at 26% 90%,rgba(210,200,176,0.10) 0%,rgba(210,200,176,0) 74%),radial-gradient(74% 64% at 92% 2%,rgba(12,14,10,0.30) 0%,rgba(12,14,10,0) 72%),#3a3d36!important}
.hero::after{background:radial-gradient(76% 48% at 44% 118%,rgba(200,190,160,0.20) 0%,rgba(190,180,150,0.07) 44%,rgba(180,170,140,0) 86%),linear-gradient(180deg,rgba(200,190,160,0) 56%,rgba(195,185,155,0.024) 78%,rgba(200,190,160,0.060) 100%)!important}
</style>`
  };

  // Variant-specific replacements extracted from the original TSX
  function getReplacements(v: Variant): [string, string, string][] {
    if (v === 'sakura-sunset') return [
      ["'  face = vec3(0.330, 0.560, 0.042);',\n        '  edge = vec3(0.062, 0.190, 0.014);',", "'  face = vec3(0.620, 0.330, 0.090);',\n        '  edge = vec3(0.150, 0.058, 0.030);',", "amber swallowtail"],
      ["'  wing = mix(wing * vec3(0.46, 1.14, 0.30), wing * vec3(1.34, 1.06, 0.16), shim);',", "'  wing = mix(wing * vec3(1.18, 0.72, 0.34), wing * vec3(1.42, 0.98, 0.28), shim);',", "amber diffraction"],
      ["'  lit += mix(vec3(0.86, 0.78, 0.20), vec3(0.34, 0.60, 0.12), border) * back * 0.42;',", "'  lit += mix(vec3(0.98, 0.66, 0.26), vec3(0.50, 0.22, 0.12), border) * back * 0.42;',", "sun through the wing"],
      ["poleTex = radialTexture(64, [[0, 'rgba(255,255,255,1)'], [0.35, 'rgba(236,244,224,0.5)'], [1, 'rgba(236,244,224,0)']]);", "poleTex = fallingSprite();", "leaf sprite"],
      ["var COUNT = (NARROW.matches || (window.innerWidth * window.innerHeight) < 620000) ? 1500 : 4200;", "var COUNT = (NARROW.matches || (window.innerWidth * window.innerHeight) < 620000) ? 140 : 340;", "leaf count"],
      ["      transparent: true, depthWrite: false, depthTest: true,\n      blending: THREE.AdditiveBlending,", "      transparent: true, depthWrite: false, depthTest: true,\n      blending: THREE.NormalBlending,", "leaves blend as leaves"],
      ["        'attribute vec4 seed;',\n        'uniform float uTime, uSize, uScale;',\n        'varying float vFade;',", "        'attribute vec4 seed;',\n        'uniform float uTime, uSize, uScale;',\n        'varying float vFade;',\n        'varying float vSpin;',\n        'varying float vFlip;',", "leaf spin and tumble varyings"],
      ["'  p.x += sin(uTime * sp * 0.35 + ph) * 34.0 * am;',", "'  p.x += sin(uTime * sp * 0.30 + ph) * 96.0 * am + sin(uTime * sp * 1.15 + ph * 2.7) * 26.0 * am;',\n        '  p.z += sin(uTime * sp * 0.41 + ph * 1.9) * 44.0 * am;',", "leaf flutter"],
      ["'  float climb = mod(uTime * 11.0 * sp + ph * 60.0, 1500.0) - 750.0;',", "'  float climb = 750.0 - mod(uTime * 11.0 * sp + ph * 90.0, 1500.0);',", "leaves fall, and slowly"],
      ["'  float twinkle = 0.55 + 0.45 * sin(uTime * (0.7 + sp * 1.6) + ph * 3.1);',", "'  vSpin = uTime * (0.26 + sp * 0.62) + ph * 2.4;',\n        '  vFlip = uTime * (0.55 + sp * 1.25) + ph * 3.7;',\n        '  float twinkle = 0.88 + 0.12 * sin(uTime * (0.7 + sp * 1.6) + ph * 3.1);',", "leaf spin and tumble"],
      ["        'uniform sampler2D uMap;',\n        'varying float vFade;',\n        'void main(){',\n        '  vec4 t = texture2D(uMap, gl_PointCoord);',\n        '  gl_FragColor = vec4(t.rgb, t.a * vFade * 0.52);',", "        'uniform sampler2D uMap;',\n        'varying float vFade;',\n        'varying float vSpin;',\n        'varying float vFlip;',\n        'void main(){',\n        '  float cs = cos(vSpin), sn = sin(vSpin);',\n        '  vec2 uv = gl_PointCoord - 0.5;',\n        '  uv = vec2(uv.x * cs - uv.y * sn, uv.x * sn + uv.y * cs);',\n        '  uv.x /= max(0.24, abs(cos(vFlip)));',\n        '  uv += 0.5;',\n        '  if (uv.x < 0.0 || uv.x > 1.0 || uv.y < 0.0 || uv.y > 1.0) discard;',\n        '  vec4 t = texture2D(uMap, uv);',\n        '  gl_FragColor = vec4(t.rgb, t.a * vFade * 0.94);',", "tumbling leaves"],
      ["uSize: { value: 9 },", "uSize: { value: 62 },", "leaf size"],
      ["motes.material.uniforms.uSize.value = Math.max(5, 9 * u);", "motes.material.uniforms.uSize.value = Math.max(30, 70 * u);", "leaf size on resize"],
      ["'  vec3 col = mix(vec3(0.30, 0.72, 0.46), vec3(0.86, 1.00, 0.90), rim);',", "'  vec3 col = mix(vec3(0.96, 0.52, 0.18), vec3(1.00, 0.90, 0.60), rim);',", "scan-light palette"]
    ];
    if (v === 'maple-autumn') return [
      ["'  face = vec3(0.330, 0.560, 0.042);',\n        '  edge = vec3(0.062, 0.190, 0.014);',", "'  face = vec3(0.820, 0.340, 0.062);',\n        '  edge = vec3(0.320, 0.080, 0.020);',", "red monarch"],
      ["'  wing = mix(wing * vec3(0.46, 1.14, 0.30), wing * vec3(1.34, 1.06, 0.16), shim);',", "'  wing = mix(wing * vec3(1.12, 0.68, 0.22), wing * vec3(1.30, 0.86, 0.18), shim);',", "autumn diffraction"],
      ["'  lit += mix(vec3(0.86, 0.78, 0.20), vec3(0.34, 0.60, 0.12), border) * back * 0.42;',", "'  lit += mix(vec3(0.96, 0.60, 0.14), vec3(0.44, 0.14, 0.06), border) * back * 0.42;',", "sun through wing"],
      ["poleTex = radialTexture(64, [[0, 'rgba(255,255,255,1)'], [0.35, 'rgba(236,244,224,0.5)'], [1, 'rgba(236,244,224,0)']]);", "poleTex = mapleSprite();", "leaf sprite"],
      ["var COUNT = (NARROW.matches || (window.innerWidth * window.innerHeight) < 620000) ? 1500 : 4200;", "var COUNT = (NARROW.matches || (window.innerWidth * window.innerHeight) < 620000) ? 120 : 280;", "leaf count"],
      ["      transparent: true, depthWrite: false, depthTest: true,\n      blending: THREE.AdditiveBlending,", "      transparent: true, depthWrite: false, depthTest: true,\n      blending: THREE.NormalBlending,", "leaves blend as leaves"],
      ["        'attribute vec4 seed;',\n        'uniform float uTime, uSize, uScale;',\n        'varying float vFade;',", "        'attribute vec4 seed;',\n        'uniform float uTime, uSize, uScale;',\n        'varying float vFade;',\n        'varying float vSpin;',\n        'varying float vFlip;',", "leaf spin and tumble varyings"],
      ["'  p.x += sin(uTime * sp * 0.35 + ph) * 34.0 * am;',", "'  p.x += sin(uTime * sp * 0.18 + ph) * 72.0 * am + sin(uTime * sp * 0.62 + ph * 2.1) * 18.0 * am;',\n        '  p.z += sin(uTime * sp * 0.28 + ph * 1.4) * 34.0 * am;',", "leaf flutter"],
      ["'  float climb = mod(uTime * 11.0 * sp + ph * 60.0, 1500.0) - 750.0;',", "'  float climb = 750.0 - mod(uTime * 7.0 * sp + ph * 90.0, 1500.0);',", "leaves fall, and slowly"],
      ["'  float twinkle = 0.55 + 0.45 * sin(uTime * (0.7 + sp * 1.6) + ph * 3.1);',", "'  vSpin = uTime * (0.18 + sp * 0.44) + ph * 2.4;',\n        '  vFlip = uTime * (0.38 + sp * 0.88) + ph * 3.7;',\n        '  float twinkle = 0.78 + 0.22 * sin(uTime * (0.5 + sp * 1.1) + ph * 2.6);',", "leaf spin and tumble"],
      ["        'uniform sampler2D uMap;',\n        'varying float vFade;',\n        'void main(){',\n        '  vec4 t = texture2D(uMap, gl_PointCoord);',\n        '  gl_FragColor = vec4(t.rgb, t.a * vFade * 0.52);',", "        'uniform sampler2D uMap;',\n        'varying float vFade;',\n        'varying float vSpin;',\n        'varying float vFlip;',\n        'void main(){',\n        '  float cs = cos(vSpin), sn = sin(vSpin);',\n        '  vec2 uv = gl_PointCoord - 0.5;',\n        '  uv = vec2(uv.x * cs - uv.y * sn, uv.x * sn + uv.y * cs);',\n        '  uv.x /= max(0.24, abs(cos(vFlip)));',\n        '  uv += 0.5;',\n        '  if (uv.x < 0.0 || uv.x > 1.0 || uv.y < 0.0 || uv.y > 1.0) discard;',\n        '  vec4 t = texture2D(uMap, uv);',\n        '  gl_FragColor = vec4(t.rgb, t.a * vFade * 0.94);',", "tumbling leaves"],
      ["uSize: { value: 9 },", "uSize: { value: 54 },", "leaf size"],
      ["motes.material.uniforms.uSize.value = Math.max(5, 9 * u);", "motes.material.uniforms.uSize.value = Math.max(24, 60 * u);", "leaf size on resize"],
      ["'  vec3 col = mix(vec3(0.30, 0.72, 0.46), vec3(0.86, 1.00, 0.90), rim);',", "'  vec3 col = mix(vec3(0.86, 0.42, 0.10), vec3(1.00, 0.82, 0.42), rim);',", "scan-light palette"]
    ];
    if (v === 'sequoia-mist') return [
      ["'  face = vec3(0.330, 0.560, 0.042);',\n        '  edge = vec3(0.062, 0.190, 0.014);',", "'  face = vec3(0.400, 0.340, 0.240);',\n        '  edge = vec3(0.120, 0.090, 0.060);',", "sequoia bark"],
      ["'  wing = mix(wing * vec3(0.46, 1.14, 0.30), wing * vec3(1.34, 1.06, 0.16), shim);',", "'  wing = mix(wing * vec3(0.92, 0.88, 0.72), wing * vec3(1.14, 1.08, 0.86), shim);',", "pale diffraction"],
      ["'  lit += mix(vec3(0.86, 0.78, 0.20), vec3(0.34, 0.60, 0.12), border) * back * 0.42;',", "'  lit += mix(vec3(0.80, 0.76, 0.56), vec3(0.32, 0.42, 0.24), border) * back * 0.42;',", "sun through wing"],
      ["poleTex = radialTexture(64, [[0, 'rgba(255,255,255,1)'], [0.35, 'rgba(236,244,224,0.5)'], [1, 'rgba(236,244,224,0)']]);", "poleTex = radialTexture(64, [[0, 'rgba(255,255,255,1)'], [0.35, 'rgba(220,210,180,0.5)'], [1, 'rgba(220,210,180,0)']]);", "mote sprite"],
      ["var COUNT = (NARROW.matches || (window.innerWidth * window.innerHeight) < 620000) ? 1500 : 4200;", "var COUNT = (NARROW.matches || (window.innerWidth * window.innerHeight) < 620000) ? 900 : 2400;", "mote count"],
      ["      transparent: true, depthWrite: false, depthTest: true,\n      blending: THREE.AdditiveBlending,", "      transparent: true, depthWrite: false, depthTest: true,\n      blending: THREE.NormalBlending,", "motes blend as dust"],
      ["'  vec3 col = mix(vec3(0.30, 0.72, 0.46), vec3(0.86, 1.00, 0.90), rim);',", "'  vec3 col = mix(vec3(0.66, 0.62, 0.46), vec3(0.92, 0.90, 0.78), rim);',", "scan-light palette"]
    ];
    return [];
  }

  const CLOSE_SCRIPT = '<' + '/script>';

  function replaceRequired(source: string, needle: string, replacement: string, label: string): string {
    if (!source.includes(needle)) {
      console.warn(`Sylva adapter: required replacement "${label}" not found.`);
      return source;
    }
    return source.replace(needle, replacement);
  }

  function buildSceneDocument(reduced: boolean, v: Variant): string {
    const presentationStart = innerGreenSource.indexOf('<main class="hero" id="hero">');
    const runtimeStart = innerGreenSource.indexOf('<script src="inner-green-assets/three.min.js">');

    if (presentationStart < 0 || runtimeStart < 0 || runtimeStart <= presentationStart) {
      throw new Error("Sylva scene adapter could not isolate the authored Three.js scene.");
    }

    let documentSource = `${innerGreenSource.slice(0, presentationStart)}${SCENE_ONLY_MARKUP(VARIANT_LABELS[v])}\n\n${innerGreenSource.slice(runtimeStart)}`
      .replace("<title>Sylva — Into the living world</title>", `<title>${VARIANT_LABELS[v]}</title>`)
      .replace("</head>", `${SCENE_ONLY_STYLE}${VARIANT_STYLES[v] ?? ""}</head>`)
      .replace(
        '<script src="inner-green-assets/three.min.js">',
        `<script data-threeui-three-runtime>${threeRuntime}${CLOSE_SCRIPT}`,
      );

    const replacements = getReplacements(v);
    if (replacements.length > 0) {
      documentSource = replacements.reduce(
        (src, [needle, replacement, label]) => replaceRequired(src, needle, replacement, label),
        documentSource,
      );
    }

    if (reduced) {
      documentSource = documentSource.replace(
        "(function loop() { requestAnimationFrame(loop); tick(); })();",
        "(function loop() { if (!REDUCED) requestAnimationFrame(loop); tick(); })();",
      );
    }

    return documentSource;
  }

  let hostEl: HTMLDivElement;
  let hostVisible = true;
  let documentVisible = true;
  let reducedMotion = false;
  let ready = false;

  let srcDoc = '';
  const background = VARIANT_BACKGROUNDS[variant];

  onMount(() => {
    const ro = new IntersectionObserver(([entry]) => {
      hostVisible = entry?.isIntersecting ?? true;
    });
    ro.observe(hostEl);

    const onVis = () => { documentVisible = !document.hidden; };
    document.addEventListener('visibilitychange', onVis);

    const media = window.matchMedia('(prefers-reduced-motion: reduce)');
    reducedMotion = media.matches;
    const onMedia = () => { reducedMotion = media.matches; };
    media.addEventListener('change', onMedia);

    documentVisible = !document.hidden;
    srcDoc = buildSceneDocument(reducedMotion, variant);

    return () => {
      ro.disconnect();
      document.removeEventListener('visibilitychange', onVis);
      media.removeEventListener('change', onMedia);
    };
  });
</script>

<div
  bind:this={hostEl}
  class="threeui-background sylva-living-world-scene {className}"
  role="img"
  aria-label="{VARIANT_LABELS[variant]} with ferns, flowers, pollen, and a butterfly"
  data-variant={variant}
  data-state={ready ? 'ready' : 'loading'}
  style="background: {background}; pointer-events: auto;"
>
  {#if srcDoc}
    <iframe
      title={VARIANT_LABELS[variant]}
      srcdoc={srcDoc}
      sandbox="allow-scripts allow-same-origin"
      loading="eager"
      on:load={(e) => { 
        ready = true;
        try {
            const iframeWin = (e.target as HTMLIFrameElement).contentWindow;
            if (iframeWin) {
                iframeWin.addEventListener('mousemove', (me: MouseEvent) => {
                    const evt = new MouseEvent('mousemove', {
                        clientX: me.clientX,
                        clientY: me.clientY,
                        bubbles: true,
                        cancelable: true,
                    });
                    window.dispatchEvent(evt);
                });
            }
        } catch (err) {}
      }}
      style="position: absolute; inset: 0; display: block; width: 100%; height: 100%; border: 0; background: {background};"
    ></iframe>
  {/if}
</div>

<style>
  .threeui-background {
    position: relative;
    width: 100%;
    height: 100%;
  }
  :global(.sylva-living-world-scene[data-state="loading"]) {
    opacity: 0;
    transition: opacity 1.4s ease;
  }
  :global(.sylva-living-world-scene[data-state="ready"]) {
    opacity: 1;
  }
</style>
