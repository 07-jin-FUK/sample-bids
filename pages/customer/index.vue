<template>
    <Header />
    <Sidebar />
    <AuctionIndex />

</template>

<script setup>
import Header from '~/components/Header.vue'
import Sidebar from '~/components/Sidebar.vue'
import AuctionIndex from '~/pages/customer/auction/index.vue' // 入札会一覧を直接読み込み

import { ref } from 'vue'
const userName = ref('')


onMounted(async () => {
  const token = useCookie('auth_token') // または useStorage

  if (token.value) {
    const { data, error } = await useFetch('/auth/me', {
      headers: {
        Authorization: `Bearer ${token.value}`
      }
    })

    if (data.value) {
      userName.value = data.value.name
    }
  }
})

</script>

<style scoped lang="scss">
.page-wrap {
  display: flex;
  flex-direction: column;
}

.dashboard-layout {
  display: flex;
  flex: 1;
}

.main-content {
  flex: 1;
  padding: 40px;
  margin-left: 200px;
  h2{
    width: 100%;
    text-align: end;
  }
}

.description {
  margin: 10px 0 30px;
  color: #555;
}
</style>
