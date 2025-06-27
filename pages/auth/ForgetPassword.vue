<template>
  <div class="page-wrap">
    <Header />
    <main class="main">
 <div class="new-comer-text">
        <h2 class="title-bottom">パスワード変更</h2>
        <p class="first-text">
           <span>パスワード再設定用のURLを添付いたします。</span>
           <span>登録したメールアドレスを入力してください。</span>
        </p>

       <form @submit.prevent="handleResetRequest">
          <div class="form-content">
            <label class="label" for="email">メールアドレス</label>
            <input type="email" id="email" v-model="email" required />
          </div>
       
          <div class="button-center">
            <button type="submit" class="button">送信</button>
          </div>
        </form>
      </div>
    </main>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import Header from '~/components/Header.vue'
import { useRouter } from 'vue-router' // 追加

const router = useRouter() // 追加
const email = ref('')
const password = ref('')

const handleResetRequest = async () => {
  try {
    const { data, error } = await useFetch('/auth/password/remind', {
      method: 'POST',
      body: {
        contact_email: email.value
      }
    })

    if (error.value) {
      alert('送信に失敗しました。メールアドレスをご確認ください。')
      return
    }

    alert('パスワードリセット用のURLを送信しました。メールをご確認ください。')
  } catch (err) {
    console.error('リマインダ送信エラー:', err)
    alert('通信エラーが発生しました。')
  }
}
</script>

<style lang="scss" scoped>
.main {
display: flex;
justify-content: center;
  margin:  150px auto;
}

.new-comer-text {
  width: 430px;
  height: 240px;
  background-color: #fff;
  padding: 50px 60px;
}

.title-bottom {
  margin-bottom: 30px;
  text-align: center;
  font-weight: normal;
  letter-spacing: 0.6px;
  font-size: 1.25em;
}

.first-text {
  font-size: 0.875em;
  text-align: center;
  span {
    display: block;
    margin-top: 10px;
    white-space: nowrap; 
  }
}

form {
  margin-top: 30px;
}

.page-wrap {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
  background-image: 
  linear-gradient(rgba(0, 0, 0, 0.4), rgba(0, 0, 0, 0.4)),
  url('/images/img-auth-common-use.jpg');
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
}

.form-content {
  margin-bottom: 20px;
}

.form-content-second {
  margin-bottom: 25px;
}

.password-text {
  font-size: 0.6875em;
  margin-top: 5px;
  color: #414141;
}

label{
    margin-left: 22px;
}

input {
  width: 81%;
  padding: 2.5px 15px;
  font-size: 1em;
  margin-left: 22px;
  border: 1px solid #707070;
  border-radius: 5px;
}

.button-center{
    text-align: center;
    margin-top: 30px;
    margin-bottom: 10px;
button {
    text-align: center;
    font-size: 0.75em;
    color: #fff;
    background-color: #000;
    padding: 10px 20px;
    border-radius: 5px;
    letter-spacing: 0.36px;
    line-height: 1;
  }
}


.button:hover {
  opacity: 0.8;
}

.sub-link {
  display: block;
  margin-top: 20px;
  font-size: 0.9em;
  color: #007bff;
  text-align: center;
  text-decoration: underline;
}

.forget-link {
margin-top: 20px;
font-size: 0.75em;
text-align: center;
}

.login-link {
  color: #4d00ff;
  font-weight: bold;
}

</style>
