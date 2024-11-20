<script setup>
const { data } = await useFetch('/api/olympians')

const activeItem = ref(null)

const setActiveItem = (id, year) => {
  const item = data.value.find((item) => item.athlete.slug === id && item.year === year)
  activeItem.value = item
}
</script>
<template>
  <div :class="activeItem && 'lg:flex lg:gap-12 lg:py-12'">
    <div class="relative transition-all duration-300"
      :class="activeItem && 'lg:w-72 lg:max-h-[740px] lg:-my-4 lg:px-4 overflow-scroll snap-y scrollbar-hidden'"
    >
      <ul
        class="flex gap-6 py-4"
        :class="activeItem ? 'lg:flex-col' : 'overflow-scroll scrollbar-hidden snap-x'"
      >
        <li v-for="item in data" class="aspect-[5/7.5] relative basis-40 shrink-0 snap-center">
          <button @click="setActiveItem(item.athlete.slug, item.year)" class="w-full h-full overflow-hidden duration-500 rounded-2xl group focus:outline-dotted outline-offset-2 outline-2">
            <NuxtImg :src="`/images/${item.athlete.image}`" height="320" width="213"
              class="overflow-hidden transition-all duration-300 scale-110 object-fit group-hover:scale-100 grayscale group-hover:grayscale-0 group-focus:grayscale-0 rounded-2xl"
              loading="lazy" />
            <p
              class="absolute right-0 p-2 text-xl text-right text-gray-900 transition-colors duration-500 bg-white/90 font-display rounded-tl-2xl rounded-br-2xl group-hover:bg-white"
              :class="activeItem ? 'bottom-0' : 'bottom-1.5'"
            >{{ item.year }}</p>
          </button>
        </li>
      </ul>
    </div>
    <div v-if="activeItem" :key="`${activeItem.athlete.slug}${activeItem.year}`" class="relative z-10 w-full mb-12">
      <div class="flex flex-col w-full gap-12 lg:flex-row">
        <div class="basis-1/3 shrink-0">
          <div
            v-motion
            :initial="{
              translateY: 10,
              scaleY: 0.95,
              opacity: 0,
            }"
            :enter="{
              translateY: 0,
              scaleY: 1,
              opacity: 1,
              transition: {
                duration: 800,
                type: 'keyframes',
                ease: 'easeInOut',
              },
            }"
            class="aspect-[5/7.5] ">
            <NuxtImg :src="`/images/${activeItem.athlete.image}`"
              class="object-fit rounded-2xl"
            />
          </div>
        </div>
        <div class="relative basis-2/3">
          <div class="max-w-[75ch] text-gray-100">
            <h1
              v-motion
              :initial="{
                opacity: 0,
                translateY: 20,
              }"
              :enter="{
                opacity: 1,
                translateY: 0,
                transition: {
                  duration: 500,
                  type: 'keyframes',
                  ease: 'easeIn',
                },
              }"
              class="font-display leading-[1.1]"><span class="block text-[90px] lg:text-[160px] text-gray-700">{{ activeItem.year }}</span> <span class="block text-5xl lg:text-[80px]">{{ activeItem.athlete.sport }}</span></h1>
            <h2 class="mb-8 text-2xl lg:text-4xl font-display"
              v-motion
              :initial="{
                opacity: 0,
                translateY: 20,
              }"
              :enter="{
                opacity: 1,
                translateY: 0,
                transition: {
                  duration: 600,
                  type: 'keyframes',
                  ease: 'easeIn',
                },
              }"
            >{{ activeItem.athlete.name }} - {{  activeItem.athlete.country }}</h2>

            <ul
              class="flex gap-2 mb-2"
              v-motion
              :initial="{ opacity: 0, x: -20 }"
              :enter="{ opacity: 1, x: 0, transition: { stagger: 0.1, ease: 'ease-out' } }"
            >
              <li v-for="i in activeItem.athlete.medals.gold" :key="`gold-${i}`">
                <svg-gold-medal class="w-6 h-6" />
              </li>
            </ul>
            <ul
              class="flex gap-2 mb-2"
              v-motion
              :initial="{ opacity: 0, x: 20 }"
              :enter="{ opacity: 1, x: 0, transition: { stagger: 0.1, ease: 'ease-out' } }"
            >
              <li v-for="i in activeItem.athlete.medals.silver" :key="`silver-${i}`">
                <svg-silver-medal class="w-6 h-6" />
              </li>
            </ul>
            <ul
              class="flex gap-2 mb-2"
              v-motion
              :initial="{ opacity: 0, scale: 0.9 }"
              :enter="{ opacity: 1, scale: 1, transition: { stagger: 0.1, ease: 'ease-out' } }"
            >
              <li v-for="i in activeItem.athlete.medals.bronze" :key="`bronze-${i}`">
                <svg-bronze-medal class="w-6 h-6" />
              </li>
            </ul>

            <p class="mb-12 text-lg leading-normal text-gray-200">{{ activeItem.athlete.story }}</p>
            <NuxtLink :to="activeItem.athlete.wikipedia" target="_blank" class="flex items-center gap-1">
              <svg-wikipedia class="w-6 h-6" /> <span>Read more on Wikipedia</span></NuxtLink>
            <div class="absolute top-0 right-0">
              <button @click="activeItem = null" class="flex items-center gap-1">
                <svg-close class="w-8 h-8 text-gray-100" /> close
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
