<script setup lang="ts">
import { ref, onMounted, reactive } from 'vue'
import axios from 'axios'

interface FormState {
  username: string
  password: string
  remember: boolean
}

const formState = reactive<FormState>({
  username: '',
  password: '',
  remember: true,
})

const onFinish = (values: FormState) => {
  console.log('Success:', values)
}

const onFinishFailed = (errorInfo: any) => {
  console.log('Failed:', errorInfo)
}

interface User {
  id: number
  name: string
  email: string
}

const users = ref<User[]>([])

const getUsers = async () => {
  try {
    const response = await axios.get<User[]>(
      'https://jsonplaceholder.typicode.com/users'
    )

    users.value = response.data

    console.log(users.value)
  } catch (error) {
    console.error('取得使用者失敗:', error)
  }
}

onMounted(() => {
  getUsers()
})
</script>

<template>
  <div>
    <a-form
      :model="formState"
      name="basic"
      :label-col="{ span: 8 }"
      :wrapper-col="{ span: 16 }"
      autocomplete="off"
      @finish="onFinish"
      @finishFailed="onFinishFailed"
    >
      <a-form-item
        label="Username"
        name="username"
        :rules="[
          {
            required: true,
            message: 'Please input your username!'
          }
        ]"
      >
        <a-input v-model:value="formState.username" />
      </a-form-item>

      <a-form-item
        label="Password"
        name="password"
        :rules="[
          {
            required: true,
            message: 'Please input your password!'
          }
        ]"
      >
        <a-input-password v-model:value="formState.password" />
      </a-form-item>

      <a-form-item
        name="remember"
        :wrapper-col="{ offset: 8, span: 16 }"
      >
        <a-checkbox v-model:checked="formState.remember">
          Remember me
        </a-checkbox>
      </a-form-item>

      <a-form-item :wrapper-col="{ offset: 8, span: 16 }">
        <a-button type="primary" html-type="submit">
          Submit
        </a-button>
      </a-form-item>
    </a-form>

    <h2>Users</h2>

    <div v-for="user in users" :key="user.id">
      <p>{{ user.name }}</p>
      <p>{{ user.email }}</p>
    </div>
  </div>
</template>