<script lang="ts">
import { defineComponent, useRoute, useRouter } from '@nuxtjs/composition-api'
import axios, { AxiosResponse, AxiosRequestConfig } from 'axios'

const URL = 'http://localhost:8555'

export default defineComponent({
  setup() {
    const route = useRoute()
    const router = useRouter()

    const fetch = async () => {
      const config: AxiosRequestConfig = {
        url: `${URL}/twitter/callback?oauth_token=${route.value.query.oauth_token}&oauth_verifier=${route.value.query.oauth_verifier}`,
        method: 'GET',
        withCredentials: true,
      }
      const res: AxiosResponse<any> = await axios.request(config)
      const { data } = res

      if (data.exist) {
        return router.push('/top')
      } else {
        return router.push('/profiles')
      }
    }
    fetch()

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
