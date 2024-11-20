<template>
  <div>
    <header class="sticky top-0 z-50 bg-white/50 backdrop-blur-md">
      <nav class="flex items-center justify-between p-4">
        <div class="flex items-center gap-2">
          <svg-olympic-logo class="h-12 text-slate-900" />
          <NuxtLink to="/" class="text-xl font-display ">Olympic Legends</NuxtLink>
        </div>
        <div class="flex gap-4">
          <NuxtLink to="#leaderboard" class="">Leaderboard</NuxtLink>
        </div>
      </nav>
    </header>
    <NuxtRouteAnnouncer />
    <NuxtPage />
  </div>
</template>
