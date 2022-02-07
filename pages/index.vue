<template>
  <div class="body-bg">
    <div class="container mx-auto px-8 py-2 columns-1">
      <div class="flex flex-wrap -mx-3 mb-6">
        <div class="w-full px-3">
          <label class="block uppercase tracking-wide text-gray-200 text-xs font-bold mb-2" for="grid-url">
            iFrame src URL
          </label>
          <input v-model="url" v-on:input="updateQuery" class="appearance-none block w-full bg-gray-100 text-gray-700 border border-gray-200 rounded py-3 px-4 mb-3 leading-tight focus:outline-none focus:bg-white focus:border-gray-500" id="grid-url" type="text">
        </div>
      </div>
      <div class="flex flex-wrap -mx-3 mb-6">
        <div class="w-full md:w-1/2 px-3 mb-6 md:mb-0">
          <label class="block uppercase tracking-wide text-gray-200 text-xs font-bold mb-2" for="grid-width">
            Width
          </label>
          <input v-model.number="width" v-on:input="updateQuery" class="appearance-none block w-full bg-gray-100 text-gray-700 border border-gray-200 rounded py-3 px-4 leading-tight focus:outline-none focus:bg-white focus:border-gray-500" id="grid-width" type="number">
        </div>
        <div class="w-full md:w-1/2 px-3">
          <label class="block uppercase tracking-wide text-gray-200 text-xs font-bold mb-2" for="grid-height">
            Height
          </label>
          <input v-model.number="height" v-on:input="updateQuery" class="appearance-none block w-full bg-gray-100 text-gray-700 border border-gray-200 rounded py-3 px-4 leading-tight focus:outline-none focus:bg-white focus:border-gray-500" id="grid-height" type="number">
        </div>
      </div>

      <div class="flex justify-center h-screen">
        <div>
          <IframeHost :url="url" :width="width" :height="height" :key="updateKey" class="mx-auto border-4 border-indigo-300" />
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import Vue from 'vue'
const defaultHeight = 600
const defaultWidth = 600
// const urlDefault = "https://example.com/"

export default Vue.extend({
  name: 'IndexPage',
  data: function() {
    const urlFromQuery = this.$route.query['url']
    const widthFromQuery = Number(this.$route.query['width'])
    const heightFromQuery = Number(this.$route.query['height'])
    return {
      updateKey: 0,
      url: urlFromQuery ? urlFromQuery : null,
      width: Number(widthFromQuery > 100 ? widthFromQuery : defaultWidth),
      height: Number(heightFromQuery > 100 ? heightFromQuery : defaultHeight),
    }

  },
  methods: {
    updateQuery() {
      const newQuery = {
        url: this.url,
        width: this.width,
        height: this.height
      }

      const existingQuery = this.$route.query
      const mergedQuery: {[key: string]: any;} = { ...existingQuery, ...newQuery }
      const queryString: string = Object.keys(mergedQuery).map((key) => {
        return encodeURIComponent(key) + '=' + encodeURIComponent(mergedQuery[key])
      }).join('&');
      if (typeof window === 'object') {
        window.history.pushState({},'',`${this.$route.path}?${queryString}`)
      }
    }
  },
  mounted() {
    this.$nextTick(function () {
      // force a redraw by updating the key
      this.updateKey = this.updateKey+1
    })

  }
})
</script>


<style scoped>
  .body-bg {
      background-color: #9921e8;
      background-image: linear-gradient(315deg, #9921e8 0%, #5f72be 74%);
  }

</style>
