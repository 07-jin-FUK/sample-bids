<template>
  <div class="page-wrap">
    <Header />
    <div class="pre-resister">
      <div class="new-comer-flow">
        <p class="top-text">まずは仮アカウントを作成してください</p>
        <div class="step-container">
          <div class="step-section">
            <div class="step-left">
              <p class="step-text">
                Step
                <span class="step-number">01</span>
              </p>
            </div>
            <div class="circle active"></div>
            <div class="text">
              <p class="desc">メールアドレス入力</p>
            </div>
          </div>
          <div class="step-section">
            <div class="step-left">
              <p class="step-text">
                Step
                <span class="step-number">02</span>
              </p>
            </div>
            <div class="circle"></div>
            <div class="text">
              <p class="desc">確認メール送付</p>
            </div>
          </div>
          <div class="step-section">
            <div class="step-left">
              <p class="step-text">
                Step
                <span class="step-number">03</span>
              </p>
            </div>
            <div class="circle"></div>
            <div class="text">
              <p class="desc bottom-text">登録申請             <span class="bottom-detail">個人情報を入力後、簡単な審査があります</span></p>
 
            </div>
          </div>
          <div class="step-section">
            <div class="step-left">
              <p class="step-text">
                Step
                <span class="step-number">04</span>
              </p>
            </div>
            <div class="circle"></div>
            <div class="text">
              <p class="desc">登録完了</p>
            </div>
          </div>
        </div>
      </div>
      <div class="new-comer-text">
        <h2 class="title-bottom">新規アカウント申請</h2>
        <p class="first-text">
          初めてご利用の方は、新規アカウント申請が必要です。
          <span>
            アカウントをお持ちの方は、<NuxtLink to="/auth/login" class="login-link">こちらからログイン</NuxtLink>できます。
          </span>
        </p>

        <form class="form" @submit.prevent="handleRegister">
          <div class="form-content">
            <label class="label" for="email">メールアドレス</label>
            <input type="email" id="email" v-model="email" required />
          </div>
          <div class="form-content-second">
            <label class="label" for="password">パスワード</label>
            <input type="password" id="password" v-model="password" required />
            <p class="password-text">6文字以上で入力してください。英小文字、数字を含める必要があります。</p>
          </div>
          <div class="form-content-second">
            <label class="checkbox-section">
              <input type="checkbox" v-model="agree" required />
              <p>利用規約、プライバシーポリシーに同意します。</p>
            </label>
          </div>
          <div class="button-left">
            <button type="submit" class="button">アカウント登録を送る</button>
          </div>
        </form>
        <p class="notice-text">※仮アカウント登録後、アカウント登録申請が必要です。</p>
      </div>
    </div>

    <!-- モーダル -->
    <div v-if="showModal" class="modal-overlay">
      <div class="modal-content">
        <p>
          仮登録処理をさせていただきました。
          <br />
          ご入力いただいたメールアドレス宛に
          <br />
          数日以内にご案内をお送りいたします。
          <br />
          今しばらくお待ちくださいませ。
        </p>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";
import { useRouter } from "vue-router";
import Header from "~/components/Header.vue";
import Footer from "~/components/Footer.vue";

const email = ref("");
const password = ref("");
const showModal = ref(false);
const router = useRouter();

const handleRegister = async () => {
  try {
    const { data, error } = await useFetch('/auth/register', {
      method: 'POST',
      body: {
        contact_email: email.value,
        password: password.value
      }
    })

    if (error.value) {
      alert('仮登録に失敗しました。入力内容をご確認ください。')
      return
    }

    showModal.value = true

    // 認証メール送信案内のモーダルを表示してから画面遷移
    setTimeout(() => {
      showModal.value = false
      router.push('/auth/activate') // token付きURLはメールに記載される前提
    }, 3000)

  } catch (err) {
    console.error('仮登録エラー', err)
    alert('通信エラーが発生しました。')
  }
}
</script>

<style lang="scss" scoped>
.new-comer-text {
  width: 385px;
  height: 380px;
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

input {
  width: 90%;
  padding: 2.5px 15px;
  font-size: 1em;
  border: 1px solid #707070;
  border-radius: 5px;
}

.button-left {
  text-align: left;
  button {
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

/* モーダルスタイル */
.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.6);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 999;
}

.modal-content {
  background: white;
  padding: 30px 40px;
  border-radius: 8px;
  text-align: center;
  font-weight: bold;
  font-size: 1em;
  line-height: 1.6;
}

.pre-resister {
  width: 100%;
  max-width: 1280px;
  display: flex;
  justify-content: center;
  align-items: center;
  margin: 0 auto;
  margin-top: 90px;
  gap: 130px;
}

.new-comer-flow {
  width: 430px;
  color: #fff;
  font-family: sans-serif;

  .top-text {
    margin-bottom: 30px;
  }
  .step-container {
    position: relative;

    .step-section {
      display: flex;
      align-items: center;
      position: relative;
      height: 32px;
      margin-bottom: 70px;

      .step-left {
        width: 100px;
        font-weight: bold;
        .step-text {
          font-size: 1.25em;
          .step-number {
            font-size: 1.389em;
          }
        }
      }
    }
  }

  .step-section:last-child {
    margin-bottom: 0;
  }
  .circle {
    width: 24px;
    height: 24px;
    border: 3px solid #fff;
    border-radius: 50%;
    text-align: center;
    line-height: 30px;
    font-weight: bold;
    margin: 0 15px 0 10px;
    background-color: transparent;
    position: relative;
    z-index: 1;
    color: #fff;
    
    &::after {
    content: "";
    position: absolute;
    top: 100%;
    left: 50%;
    transform: translateX(-50%);
    width: 3px;
    height: 76px; // サークル間の距離に応じて調整
    background-color: #fff;
    z-index: 0;
  }
}

  .circle.active {
    background-color: #fff;
    color: #000;
  }

.step-section:last-child .circle::after {
  display: none;
}

  .text {
    .desc {
      font-size: 1.125em;
      letter-spacing: 0.54px;
      color: #ffffff;
      text-shadow: 0px 3px 6px #00000029;
    }
    .bottom-text {
      position: relative;
    }
    .bottom-detail {
      position: absolute;
      top:35px;
      left:0;
      font-size: 0.778em;
      letter-spacing: 0.42px;
      color: #ffffff;
      text-shadow: 0px 3px 6px #00000029;
      white-space: nowrap; 
    }
  }
}

.login-link {
  color: #4d00ff;
  font-weight: bold;
}

.checkbox-section {
  display: flex;
  align-items: center;
  input {
    margin: 0;
    width: 20px;
    height: 20px;
    border: 1px solid #707070;
    border-radius: 5px;
  }

  p {
    margin-left: 10px;
    font-size: 0.75em;
    text-align: left;
    letter-spacing: 0.36px;
    color: #676767;
  }
}

.notice-text {
  font-size: 0.75em;
  color: #676767;
  margin-top: 15px;
}
</style>
