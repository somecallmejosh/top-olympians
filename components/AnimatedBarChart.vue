<script setup>
const { $gsap } = useNuxtApp()
import Chart from "chart.js/auto"

const { data } = await useFetch("/api/olympians")

function aggregateMedals(data) {
  const aggregated = {}

  data.forEach((entry) => {
    const { name, medals, image } = entry.athlete
    if (!aggregated[name]) {
      aggregated[name] = { gold: 0, silver: 0, bronze: 0, image }
    }
    aggregated[name].gold += medals.gold
    aggregated[name].silver += medals.silver
    aggregated[name].bronze += medals.bronze
  })

  return aggregated
}

const aggregatedData = aggregateMedals(data.value)

const chartData = ref({
  labels: ["Gold", "Silver", "Bronze"],
  datasets: Object.entries(aggregatedData).map(([name, medals]) => ({
    label: name,
    data: [medals.gold, medals.silver, medals.bronze],
    backgroundColor: ["#FFD700", "#C0C0C0", "#CD7F32"]
  }))
})


function getTopMedalist(aggregatedData, medalType) {
  const topMedalist = Object.entries(aggregatedData).reduce(
    (max, [name, medals]) => {
      return medals[medalType] > max.count
        ? { name, count: medals[medalType], image: medals.image }
        : max
    },
    { name: null, count: 0, image: null }
  )
  return topMedalist
}

const topGoldMedalist = ref(getTopMedalist(aggregatedData, "gold"))
const topSilverMedalist = ref(getTopMedalist(aggregatedData, "silver"))
const topBronzeMedalist = ref(getTopMedalist(aggregatedData, "bronze"))

const chartOptions = {
  responsive: true,
  maintainAspectRatio: false,
  animation: {
    duration: 2000,
    easing: "easeOutBounce"
  },
  plugins: {
    legend: {
      display: false,
    }
  }
}

const chartCanvas = ref(null)
onMounted(() => {
  $gsap.to('.title', { y: 0, opacity: 1, duration: 0.5, scrollTrigger: { trigger: '.medal', start: 'top 80%' }  })
  $gsap.to('.medalist-card ', { opacity: 1, y: 0, duration: 0.5, stagger: 0.2, scrollTrigger: { trigger: '.medal', start: 'top 80%' }  })
  new Chart(chartCanvas.value, {
    type: "bar",
    data: {
      labels: chartData.value.labels,
      datasets: chartData.value.datasets
    },
    options: chartOptions
  })
})
</script>

<template>
  <div id="leaderboard" class="relative px-4 py-8 -mx-4 space-y-6 lg:space-y-12 lg:-mx-4 lg:px-32 lg:py-20 bg-gradient-to-b from-gray-50 via-gray-100 to-gray-50 scroll-m-20">
    <h2 class="text-4xl text-gray-900 translate-y-2 opacity-0 title lg:text-7xl font-display">Top Medalists of the <br>Last Two Decades</h2>
    <div class="relative z-10 flex flex-col items-center gap-12 lg:flex-row">
      <div class="w-full space-y-6 text-gray-900 lg:w-auto">
        <ul class="grid grid-cols-3 gap-1 lg:gap-4 lg:grid-cols-1">
          <li class="p-2 translate-y-10 rounded-lg opacity-0 medalist-card bg-gray-200/50">
            <div class="flex flex-col items-center gap-2 lg:flex-row">
              <div class="w-20">
                <div class="overflow-hidden aspect-[1/1] rounded-full border-2 border-yellow-400">
                  <NuxtImg :src="`/images/${topGoldMedalist.image}`" class="object-fit" />
                </div>
              </div>
              <div class="space-y-1 text-center">
                <p class="text-xs sm:text-sm">{{ topGoldMedalist.name }}</p>
                <p class="flex items-center gap-1 text-xs"><svg-gold-medal class="hidden w-4 h-4 sm:block" />{{ topGoldMedalist.count }} gold medals</p>
              </div>
            </div>
          </li>
          <li class="p-2 translate-y-10 rounded-lg opacity-0 medalist-card bg-gray-200/50">
             <div class="flex flex-col items-center gap-2 lg:flex-row">
              <div class="w-20">
                <div class="overflow-hidden aspect-[1/1] rounded-full border-2 border-gray-300">
                  <NuxtImg :src="`/images/${topSilverMedalist.image}`" class="object-fit" />
                </div>
              </div>
              <div class="space-y-1 text-center">
                <p class="text-xs sm:text-sm">{{ topSilverMedalist.name }}</p>
                <p class="flex items-center gap-1 text-xs">
                  <svg-silver-medal class="hidden w-4 h-4 sm:block" />{{ topSilverMedalist.count }} silver medals</p>
              </div>
            </div>
          </li>
          <li class="p-2 translate-y-10 rounded-lg opacity-0 medalist-card bg-gray-200/50">
             <div class="flex flex-col items-center gap-2 lg:flex-row">
              <div class="w-20">
                <div class="overflow-hidden aspect-[1/1] rounded-full border-2 border-orange-600">
                  <NuxtImg :src="`/images/${topBronzeMedalist.image}`" class="object-fit" />
                </div>
              </div>
              <div class="space-y-1 text-center">
                <p class="text-xs sm:text-sm">{{ topBronzeMedalist.name }}</p>
                <p class="flex items-center gap-1 text-xs"><svg-bronze-medal class="hidden w-4 h-4 sm:block" />{{ topBronzeMedalist.count }} bronze medals</p>
              </div>
            </div>
          </li>
        </ul>
      </div>
      <div class="flex-1">
        <div>
          <canvas ref="chartCanvas" />
        </div>
      </div>
    </div>
  </div>
</template>

<style>
canvas {
  max-width: 100%;
  height: 400px;
}
</style>
