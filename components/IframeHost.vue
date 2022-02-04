<template>
  <iframe :width="width" :height="height" :src="url" />
</template>

<script lang="ts">
import Vue from 'vue'

export default Vue.extend({
  name: 'IframeHost',
  props: {
    url: String,
    height: {
      type: Number,
      default: null
    },
    width: {
      type: Number,
      default: null
    }
  }
})
</script>
