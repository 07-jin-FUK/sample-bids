<template>
  <div class="page-wrap">
    <Header />
      <Sidebar />
    <main class="main">

      <h2 class="title">プロフィール編集</h2>

      <form class="form" @submit.prevent="handleSubmit">
        <div class="form-group">
          <label for="company">会社名</label>
          <input type="text" id="company" v-model="profile.company" required />
        </div>

        <div class="form-group">
          <label for="person">担当者名</label>
          <input type="text" id="person" v-model="profile.person" required />
        </div>

        <div class="form-group">
          <label for="email">メールアドレス</label>
          <input type="email" id="email" v-model="profile.email" required />
        </div>

        <div class="form-group">
          <label for="phone">電話番号</label>
          <input type="tel" id="phone" v-model="profile.phone" />
        </div>

        <div class="form-group">
          <label for="address">住所</label>
          <input type="text" id="address" v-model="profile.address" />
        </div>

        <div class="button-wrap">
          <button type="submit" class="button">保存する</button>
        </div>
      </form>
    </main>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import Header from '~/components/Header.vue'
import Sidebar from '~/components/Sidebar.vue'


import { onMounted } from 'vue'
import { useCookie } from '#app' // Nuxt3でCookie取得

onMounted(async () => {
  const token = useCookie('auth_token')

  if (token.value) {
    const { data, error } = await useFetch('/auth/me', {
      headers: {
        Authorization: `Bearer ${token.value}`
      }
    })

    if (data.value) {
      profile.value = {
        company: data.value.company,
        person: data.value.person,
        email: data.value.email,
        phone: data.value.phone,
        address: data.value.address,
      }
    }
  }
})

const handleSubmit = async () => {
  const token = useCookie('auth_token')
  const { error } = await useFetch('/auth/profile/update', {
    method: 'POST',
    headers: {
      Authorization: `Bearer ${token.value}`
    },
    body: profile.value
  })

  if (error.value) {
    alert('プロフィールの保存に失敗しました')
  } else {
    alert('プロフィールを保存しました')
  }
}
</script>

<style scoped lang="scss">
.page-wrap {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
}

.main {
  flex: 1;
  padding: 40px;
  max-width: 600px;
  margin: auto;
}

.title {
  margin-bottom: 30px;
  text-align: center;
}

.form-group {
  margin-bottom: 20px;
}

label {
  font-weight: bold;
  display: block;
  margin-bottom: 5px;
}

input {
  width: 100%;
  padding: 10px 15px;
  font-size: 1em;
  border: 1px solid #ccc;
  border-radius: 4px;
}

.button-wrap {
  text-align: center;
  margin-top: 30px;
}

.button {
  padding: 12px 24px;
  background-color: #007bff;
  color: white;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  font-weight: bold;
}

.button:hover {
  background-color: #0056b3;
}
</style>
