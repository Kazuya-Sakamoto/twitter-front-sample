<script lang="ts">
import { defineComponent, useRouter } from '@nuxtjs/composition-api'

export default defineComponent({
  setup() {
    const router = useRouter()
    const routerPush = () => {
      return router.push('/')
    }
    return {
      routerPush,
    }
  },
})
</script>

<template>
  <section class="text-gray-600 body-font">
    <div class="container px-5 py-24 mx-auto">
      <div
        class="lg:w-2/3 flex flex-col sm:flex-row sm:items-center items-start mx-auto"
      >
        <h1
          class="flex-grow sm:pr-16 text-2xl font-medium title-font text-gray-900"
        >
          exist : true だったため、/top 画面に遷移しました。
        </h1>
        <button
          class="flex-shrink-0 text-white bg-purple-500 border-0 py-2 px-8 focus:outline-none hover:bg-purple-600 rounded text-lg mt-10 sm:mt-0"
          @click="routerPush"
        >
          認証画面に戻る
        </button>
      </div>
    </div>
  </section>
</template>

<style>
.content {
  height: 100vh;
}
</style>
