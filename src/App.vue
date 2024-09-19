<script setup>
import { ref, computed } from 'vue';

// Mouse position constants
const mouseX = ref(0);
const mouseY = ref(0);
const isMenuShown = ref(false);

// Number of items in the menu
const numberOfItems = ref(11); // You can change this value to 6, 10, etc.
const radius = 100; // Radius of the circle around the mouse position

// Function to handle click and toggle the menu
function handleClick(event) {
  mouseX.value = event.clientX;
  mouseY.value = event.clientY;
  isMenuShown.value = !isMenuShown.value;
}

// Calculate the position of each item
const items = computed(() => {
  const angleStep = (2 * Math.PI) / numberOfItems.value;
  return Array.from({ length: numberOfItems.value }, (_, index) => {
    const angle = index * angleStep;
    return {
      topOffset: Math.sin(angle) * radius,
      leftOffset: Math.cos(angle) * radius
    };
  });
});
</script>

<template>
  <!-- Div covers the whole screen and listens for click events -->
  <div @click="handleClick" id="wholeScreen">

    <!-- Dynamic buttons arranged around the click point -->
    <div
      v-if="isMenuShown"
      v-for="(item, index) in items"
      :key="index"
      class="circle-button"
      :style="{
        top: mouseY + item.topOffset + 'px',
        left: mouseX + item.leftOffset + 'px',
      }"
    ></div>
  </div>
</template>
