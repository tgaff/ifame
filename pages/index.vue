<template>
  <div class="body-bg">
    <div class="container mx-auto px-8 py-2 columns-1">
      <!-- <div class="mb-6 pt-3 rounded bg-gray-200">
        <label class="block text-gray-700 text-sm font-bold mb-2 ml-3" for="url">iFrame src URL</label>
        <input v-model="url" v-on:input="updateQuery" type="text" id="url" class="bg-gray-100 rounded w-full text-gray-700 focus:outline-none border-b-4 border-gray-300 focus:border-purple-600 transition duration-500 px-3 pb-3">
      </div>

      <div class="mb-6 pt-3 rounded bg-gray-200 flex">
        <div class="w-1/2 px-2">
          <label class="block text-gray-700 text-sm font-bold mb-2 ml-3" for="width">Width</label>
          <input v-model.number="width" v-on:input="updateQuery" type="text" id="width" class="bg-gray-100 rounded w-full text-gray-700 focus:outline-none border-b-4 border-gray-300 focus:border-purple-600 transition duration-500 px-3 pb-3">
        </div>
        <div class="w-1/2 px-2">
          <label class="block text-gray-700 text-sm font-bold mb-2 ml-3" for="height">Height</label>
          <input v-model.number="height" v-on:input="updateQuery" type="text" id="height" class="bg-gray-100 rounded w-full text-gray-700 focus:outline-none border-b-4 border-gray-300 focus:border-purple-600 transition duration-500 px-3 pb-3">
        </div>
      </div>
 -->

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
          <input v-model.number="height" v-on:input="updateQuery" class="appearance-none block w-full bg-gray-100 text-gray-700 border border-gray-200 rounded py-3 px-4 leading-tight focus:outline-none focus:bg-white focus:border-gray-500" id="grid-height" type="text">
        </div>
      </div>

      <div class="flex justify-center h-screen">
        <div >
          <IframeHost :url="url" :width="width" :height="height" class="mx-auto border-4 border-indigo-300" />
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import Vue from 'vue'
const defaultHeight = 600
const defaultWidth = 600

export default Vue.extend({
  name: 'IndexPage',
  data: function() {
    const urlDefault = "https://example.com/"
    const urlFromQuery = this.$route.query['url']
    const widthFromQuery = Number(this.$route.query['width'])
    const heightFromQuery = Number(this.$route.query['height'])

    return {
      url: urlFromQuery ? urlFromQuery : urlDefault,
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
  }
})
</script>


<style scoped>
  .body-bg {
      background-color: #9921e8;
      background-image: linear-gradient(315deg, #9921e8 0%, #5f72be 74%);
  }

</style>
