<script setup>
defineProps(['country'])

const setSVG = (country) => {
  return defineAsyncComponent(() => import(`@/components/svg/${country.toLowerCase()}.vue`))
}
</script>

<template>
  <component :is="setSVG(country)" class="h-6" />
</template>
