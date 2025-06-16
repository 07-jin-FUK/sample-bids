<script setup lang="ts">
import { useRouter } from 'vue-router'
import { onMounted } from 'vue'

const router = useRouter()

onMounted(() => {
  router.push('/auth/login')
})
</script>

<template>
  <div></div> <!-- 画面に何も表示しないでOK -->
</template>
