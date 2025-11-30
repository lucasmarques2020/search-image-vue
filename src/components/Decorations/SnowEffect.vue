<script setup>
import { ref, onMounted } from 'vue';

const totalFlakes = 200;
const flakes = ref([]);

const randomRange = (min, max) => Math.floor(Math.random() * (max - min + 1)) + min;

onMounted(() => {
  flakes.value = Array.from({ length: totalFlakes }, (_, i) => {
    // Corresponde exatamente ao SCSS:
    // $random-x: random(1000000) * 0.0001vw;
    const randomX = Math.random() * 1000000 * 0.0001; // em vw
    
    // $random-offset: random_range(-100000, 100000) * 0.0001vw;
    const randomOffset = randomRange(-100000, 100000) * 0.0001; // em vw
    
    // $random-x-end: $random-x + $random-offset;
    const randomXEnd = randomX + randomOffset;
    
    // $random-x-end-yoyo: $random-x + ($random-offset / 2);
    const randomXEndYoyo = randomX + randomOffset / 2;
    
    // $random-yoyo-time: random_range(30000, 80000) / 100000;
    const randomYoyoTime = randomRange(30000, 80000) / 100000;
    
    // $random-yoyo-y: $random-yoyo-time * 100vh;
    const randomYoyoY = randomYoyoTime * 100; // em vh
    
    // $random-scale: random(10000) * 0.0001;
    const randomScale = Math.random() * 10000 * 0.0001;
    
    // $fall-duration: random_range(10, 30) * 1s;
    const fallDuration = randomRange(10, 30);
    
    // $fall-delay: random(30) * -1s;
    const fallDelay = -Math.random() * 30;
    
    // opacity: random(10000) * 0.0001;
    const opacity = Math.random() * 10000 * 0.0001;
    
    // percentage($random-yoyo-time) = randomYoyoTime * 100%
    const yoyoPercent = (randomYoyoTime * 100).toFixed(1);

    return {
      id: i,
      randomX,
      randomOffset,
      randomXEnd,
      randomXEndYoyo,
      randomYoyoTime,
      randomYoyoY,
      randomScale,
      fallDuration,
      fallDelay,
      opacity,
      yoyoPercent,
    };
  });

  // Inject dynamic keyframes into document
  injectKeyframes();
});

const flakeStyle = (flake) => {
  return {
    opacity: flake.opacity.toFixed(4),
    transform: `translate(${flake.randomX.toFixed(4)}vw, -10px) scale(${flake.randomScale.toFixed(4)})`,
    animation: `fall-${flake.id} ${flake.fallDuration}s ${flake.fallDelay.toFixed(2)}s linear infinite`,
  };
};

const injectKeyframes = () => {
  const style = document.createElement('style');
  let keyframesCSS = '';

  flakes.value.forEach((flake) => {
    keyframesCSS += `
      @keyframes fall-${flake.id} {
        ${flake.yoyoPercent}% {
          transform: translate(${flake.randomXEnd.toFixed(4)}vw, ${flake.randomYoyoY.toFixed(1)}vh) scale(${flake.randomScale.toFixed(4)});
        }
        100% {
          transform: translate(${flake.randomXEndYoyo.toFixed(4)}vw, 100vh) scale(${flake.randomScale.toFixed(4)});
        }
      }
    `;
  });

  style.textContent = keyframesCSS;
  document.head.appendChild(style);
};
</script>

<template>
  <div class="pointer-events-none absolute inset-0 overflow-hidden">
    <div
      v-for="flake in flakes"
      :key="flake.id"
      class="absolute w-2.5 h-2.5 bg-white rounded-full"
      :style="flakeStyle(flake)"
    ></div>
  </div>
</template>

<style scoped>
</style>
