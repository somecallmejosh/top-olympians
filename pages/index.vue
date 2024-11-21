<script setup></script>
<template>
  <div
    class="relative p-4 antialiased text-white bg-gradient-to-b from-gray-900 via-gray-800 to-gray-900"
  >
    <svg-olympic-logo class="h-[1600px] fixed -top-56 -right-40 text-white/5 opacity-20" />
    <CardDisplay />
    <div class="relative z-10">
      <AnimatedBarChart />
    </div>
  </div>
</template>
