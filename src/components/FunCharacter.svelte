<script>
  import { onMount } from 'svelte';
  let floatOffset = 0;
  let raf;
  onMount(() => {
    const loop = () => {
      floatOffset += 0.03;
      raf = requestAnimationFrame(loop);
    };
    raf = requestAnimationFrame(loop);
    return () => cancelAnimationFrame(raf);
  });
  $: translateY = Math.sin(floatOffset) * 6;
  $: rotate = Math.sin(floatOffset * 0.6) * 3;
</script>

<div class="astronaut" style="transform: translateY({translateY}px) rotate({rotate}deg);">
  <svg width="120" height="140" viewBox="0 0 120 140" fill="none" xmlns="http://www.w3.org/2000/svg">
    <defs>
      <linearGradient id="suit" x1="0" y1="0" x2="0" y2="1">
        <stop offset="0%" stop-color="#ffffff" stop-opacity="0.95"/>
        <stop offset="100%" stop-color="#dbeafe" stop-opacity="0.95"/>
      </linearGradient>
      <radialGradient id="visor" cx="50%" cy="50%" r="50%">
        <stop offset="0%" stop-color="#93c5fd" stop-opacity="0.9"/>
        <stop offset="100%" stop-color="#1f2937" stop-opacity="0.9"/>
      </radialGradient>
    </defs>
    <!-- Backpack -->
    <rect x="28" y="32" rx="10" width="64" height="78" fill="#94a3b8"/>
    <!-- Body -->
    <rect x="32" y="38" rx="12" width="56" height="86" fill="url(#suit)" stroke="#cbd5e1"/>
    <!-- Helmet ring -->
    <circle cx="60" cy="36" r="26" fill="#e2e8f0" stroke="#cbd5e1"/>
    <!-- Visor -->
    <ellipse cx="60" cy="36" rx="18" ry="14" fill="url(#visor)"/>
    <!-- Control panel -->
    <rect x="46" y="72" rx="6" width="28" height="20" fill="#1f2937"/>
    <circle cx="52" cy="82" r="3" fill="#ef4444"/>
    <circle cx="60" cy="82" r="3" fill="#22c55e"/>
    <circle cx="68" cy="82" r="3" fill="#3b82f6"/>
    <!-- Arms -->
    <rect x="18" y="56" rx="10" width="24" height="12" fill="url(#suit)" stroke="#cbd5e1"/>
    <rect x="78" y="56" rx="10" width="24" height="12" fill="url(#suit)" stroke="#cbd5e1"/>
    <!-- Legs -->
    <rect x="44" y="116" rx="8" width="12" height="18" fill="#e2e8f0" stroke="#cbd5e1"/>
    <rect x="64" y="116" rx="8" width="12" height="18" fill="#e2e8f0" stroke="#cbd5e1"/>
    <!-- Boots -->
    <rect x="40" y="130" rx="6" width="20" height="8" fill="#0ea5e9"/>
    <rect x="60" y="130" rx="6" width="20" height="8" fill="#0ea5e9"/>
    <!-- Tether -->
    <path d="M 96 72 C 120 64, 132 32, 100 24 C 88 20, 80 10, 88 4" stroke="#94a3b8" stroke-width="3" fill="none" stroke-linecap="round"/>
  </svg>
</div>

<style>
  .astronaut {
    width: 120px;
    height: 140px;
    filter: drop-shadow(0 8px 18px rgba(0,0,0,0.35));
    transition: transform 0.08s linear;
  }
</style>


