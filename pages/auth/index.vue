<script lang="ts">
import { defineComponent } from '@nuxtjs/composition-api'

export default defineComponent({
  setup() {
    return {}
  },
})
</script>

<template>
  <div class="content">
    <div class="p-8 flex justify-center align-middle">
      <div class="flex justify-center my-20">
        <div class="animate-spin h-8 w-8 bg-purple-500 rounded-xl" />
      </div>
    </div>
  </div>
</template>

<style>
.content {
  height: 100vh;
}
</style>
