<script lang="ts">
import {
  defineComponent,
  reactive,
  toRefs,
  useRouter,
} from '@nuxtjs/composition-api'
import axios, { AxiosResponse, AxiosRequestConfig } from 'axios'

type T = {
  State: {
    inputs: {
      name: string
      password: string
    }
    isError: boolean
  }
  Params: {
    uid: string
    password: string
  }
  RegisterRes: {
    jwtToken: string
    url: string
  }
}

const initialState = (): T['State'] => ({
  inputs: {
    name: '',
    password: '',
  },
  isError: false,
})

const URL = 'http://localhost:8555'

export default defineComponent({
  setup() {
    const state = reactive<T['State']>(initialState())
    const router = useRouter()

    const onRegister = async () => {
      const config: AxiosRequestConfig = {
        url: `${URL}/signup`,
        method: 'POST',
        params: {
          uid: state.inputs.name,
          password: state.inputs.password,
        } as T['Params'],
      }

      try {
        const res: AxiosResponse<T['RegisterRes']> = await axios.request(config)
        const { data, status } = res
        if (status === 200) {
          window.open(data.url, '_blank')
          return router.push('/auth')
        }
        state.isError = true
      } catch (e) {
        if (axios.isAxiosError(e)) {
          return e.message
        }
      }
    }

    const onAuthenticate = async () => {
      const config: AxiosRequestConfig = {
        url: `${URL}/signup`,
        method: 'POST',
        withCredentials: true,
        params: {
          callbackURL: `http://localhost:3999/app/twitter/callback`,
        },
      }
      const res: AxiosResponse<any> = await axios.request(config)
      const { data } = res
      window.location.href = data.url
    }

    return {
      ...toRefs(state),
      onRegister,
      onAuthenticate,
    }
  },
})
</script>

<template>
  <div class="content">
    <div class="p-8 flex justify-center align-middle">
      <form class="w-full max-w-sm">
        <div class="md:flex md:items-center mb-6">
          <div class="md:w-1/3">
            <label
              class="block text-gray-500 font-bold md:text-right mb-1 md:mb-0 pr-4"
              for="inline-full-name"
            >
              お名前
            </label>
          </div>
          <div class="md:w-2/3">
            <input
              v-model="inputs.name"
              class="bg-gray-200 appearance-none border-2 border-gray-200 rounded w-full py-2 px-4 text-gray-700 leading-tight focus:outline-none focus:bg-white focus:border-purple-500"
              type="text"
            />
          </div>
        </div>
        <div class="md:flex md:items-center mb-6">
          <div class="md:w-1/3">
            <label
              class="block text-gray-500 font-bold md:text-right mb-1 md:mb-0 pr-4"
              for="inline-password"
            >
              パスワード
            </label>
          </div>
          <div class="md:w-2/3">
            <input
              v-model="inputs.password"
              class="bg-gray-200 appearance-none border-2 border-gray-200 rounded w-full py-2 px-4 text-gray-700 leading-tight focus:outline-none focus:bg-white focus:border-purple-500"
              type="password"
              placeholder="******************"
            />
          </div>
        </div>
        <div class="md:flex md:items-center">
          <div class="md:w-1/3"></div>
          <div class="md:w-2/3">
            <button
              class="shadow bg-purple-500 hover:bg-purple-400 focus:shadow-outline focus:outline-none text-white font-bold py-2 px-4 rounded"
              type="button"
              @click="onRegister"
            >
              Sign Up
            </button>
          </div>
        </div>

        <div class="md:flex md:items-center pt-8">
          <div class="md:w-1/3"></div>
          <div class="md:w-2/3">
            <button
              class="shadow bg-blue-500 hover:bg-blue-400 focus:shadow-outline focus:outline-none text-white font-bold py-2 px-4 rounded"
              type="button"
              @click="onAuthenticate"
            >
              Twitter 認証
            </button>
          </div>
        </div>
      </form>
      <template v-if="isError">
        <div>
          <p>登録が失敗</p>
        </div>
      </template>
      <p></p>
    </div>
  </div>
</template>

<style>
.content {
  height: 100vh;
}
</style>
