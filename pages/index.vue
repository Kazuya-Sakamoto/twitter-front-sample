<script lang="ts">
import {
  defineComponent,
  reactive,
  toRefs,
  useRouter,
} from '@nuxtjs/composition-api'
import axios from 'axios'

type State = {
  inputs: {
    name: string
    password: string
  }
}
type Params = {
  uid: string
  password: string
}
type Response = {
  jwt_token: string
  url: string
}

const initialState = (): State => ({
  inputs: {
    name: '',
    password: '',
  },
})

const URL = 'http://localhost:8555'

export default defineComponent({
  setup() {
    const state = reactive<State>(initialState())
    const router = useRouter()

    const onRegister = async () => {
      const params: Params = {
        uid: state.inputs.name,
        password: state.inputs.password,
      }
      try {
        const res = await axios.post<Response>(`${URL}/signup`, params)
        console.log(res, 'res')
        window.open(res.data.url, '_blank')
        return router.push('/auth')
      } catch (error) {
        return error
      }
    }

    return {
      ...toRefs(state),
      onRegister,
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
      </form>
    </div>
  </div>
</template>

<style>
.content {
  height: 100vh;
}
</style>
