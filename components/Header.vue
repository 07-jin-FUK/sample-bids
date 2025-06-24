<template>
  <header class="header">
    <h1>日本中古計測器</h1>
    
        <!-- ログイン中（customer以降）に表示するアイコン -->
    <div v-if="isCustomerPage" class="header-icons">
      <NuxtLink to="/customer/notification">
        <IconNotification />
      </NuxtLink>
      <NuxtLink to="/customer/profileedit">
        <IconUser />
      </NuxtLink>
    </div>
    
    
    <div class="header-right">
      <NuxtLink v-if="showLoginButton" to="/auth/login" class="login-button">ログイン</NuxtLink>
    </div>
    <div class="header-right">
      <NuxtLink v-if="showRegisterButton" to="/auth/register" class="login-button">新規会員登録</NuxtLink>
    </div>
  </header>
</template>

<script setup>
import { useRoute } from 'vue-router'
import IconUser from '~/components/icons/IconUser.vue'      // 任意のユーザーアイコン
import IconNotification from '~/components/icons/IconNotification.vue'

const route = useRoute()

// Register ページだけで表示する
const showLoginButton = [
  '/auth/register',
  '/auth/forgetpassword',
].includes(route.path)

// Register ページだけで表示する
const showRegisterButton = [
  '/auth/login',
].includes(route.path)

// customer以降のページだったらアイコンを表示
const isCustomerPage = route.path.startsWith('/customer')

</script>

<style lang="scss" scoped>
.header {
  background-color: #fff;
  color: #000000;
  height: 80px;
  display: flex;
  justify-content: center;
  align-items: center;
  letter-spacing: 0.6px;
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  z-index: 1000;

  h1 {
    font-weight: normal;
    font-size: 1.25em;
    width: 100%;
    text-align: center;
    margin: 0 auto;
  }

  .header-right {
    position: absolute;
    right: 50px;
    top: 50%;
    transform: translateY(-50%);

    .login-button {
      background-color: #333;
      color: #fff;
      padding: 10px 20px;
      border-radius: 5px;
      text-decoration: none;
      font-size: 0.875em;
    }
  }
  
  .header-icons {
    position: absolute;
    right: 30px;
    top: 50%;
    transform: translateY(-50%);
    display: flex;
    gap: 20px;

    a {
      display: flex;
      align-items: center;
      justify-content: center;
      color: #333;

      svg {
        width: 30px;
        height: 30px;
      }
    }
  }
}

</style>
