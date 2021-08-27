<template>
  <div
    :class="isDark ? '' : 'dark'"
    class="transition duration-500 ease-in-out transform"
  >
    <div class="p-10 min-h-screen bg-blue-100 dark:bg-gray-800">
      <div class="bg-blue-300 my-2 py-4 shadow-md dark:bg-gray-600">
        <h1 class="text-lg text-center dark:text-white">
          Welcome to the frontend test for <b>Develon Group</b>! - Developed by
          <b>Nicolau Riess Selicheff</b>
        </h1>
      </div>
      <div class="flex justify-center py-4 mx-4">
        <button
          class="
            mx-2
            transition-colors
            shadow-sm
            bg-white
            hover:bg-blue-500
            text-blue-700
            font-semibold
            hover:text-white
            py-2
            px-4
            border border-blue-500
            hover:border-transparent
            rounded
            dark:bg-white
            dark:text-gray-700
            dark:border-gray-700
            dark:hover:text-white
            dark:hover:bg-gray-500
          "
          @click="asyncData"
        >
          Discover the rivers
        </button>
        <button
          class="
            mx-2
            transition-colors
            shadow-sm
            bg-white
            hover:bg-blue-500
            text-blue-700
            font-semibold
            hover:text-white
            py-2
            px-4
            border border-blue-500
            hover:border-transparent
            rounded
            dark:bg-white
            dark:text-gray-700
            dark:border-gray-700
            dark:hover:text-white
            dark:hover:bg-gray-500
          "
          @click="isDark = !isDark"
        >
          Dark Mode
        </button>
      </div>
      <div v-if="fetchedData" class="mt-4">
        <vueper-slides
          class="no-shadow center"
          :arrows-outside="false"
          bullets-outside
          transition-speed="250"
          slide-content-outside="bottom"
        >
          <vueper-slide
            v-for="(river, i) in rivers"
            :key="i"
            :river="river.river"
            :image="river.image"
          >
            <template #content
              ><div
                class="
                  bg-blue-300
                  border-1
                  p-2
                  border-blue-500
                  shadow-md
                  transition
                  duration-500
                  ease-in-out
                  transform
                  hover:-translate-y-1 hover:scale-110
                  dark:bg-gray-200
                "
              >
                <div class="vueperslide__content-wrapper">
                  <span
                    class="
                      text-white text-xs text-justify
                      dark:bg-gray-200 dark:text-black
                    "
                    >{{ river.description }}</span
                  >
                  <span
                    class="
                      text-white text-xs text-justify
                      dark:bg-gray-200 dark:text-black
                    "
                    >Length: {{ river.length }}</span
                  >
                </div>
              </div>
            </template>
          </vueper-slide>
        </vueper-slides>
      </div>
    </div>
  </div>
</template>

<script>
import { VueperSlides, VueperSlide } from 'vueperslides'
import 'vueperslides/dist/vueperslides.css'
const url = 'https://api.nuxtjs.dev/rivers'

export default {
  components: {
    VueperSlides,
    VueperSlide,
  },
  data() {
    return {
      fetchedData: false,
      titles: null,
      isDark: true,
    }
  },

  created() {},
  methods: {
    async asyncData() {
      const response = await fetch(url)
      if (!response.ok) {
        throw new Error(response.status)
      }
      const data = await response.json()

      const unorderedLen = data.map((river) => ({
        ...river,
        length: river.length.replace(/\D/g, ''),
      }))
      const orderedRivers = unorderedLen.sort((a, b) => a.length - b.length)

      const rivers = orderedRivers.map((title) => ({
        ...title,
        length: data.find((river) => river.slug === title.slug).length,
      }))

      this.rivers = rivers
      this.fetchedData = true
    },
  },
}
</script>

<style>
.center {
  width: 600px;
  max-width: 100%;
  margin: auto;
}
</style>
