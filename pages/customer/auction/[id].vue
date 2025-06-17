<template>
  <div class="page-wrap">
    <Header />
          <Sidebar />
    <main class="main">
        <p></p>
        
        
      <h2 class="title">入札会詳細（ID: {{ id }}）</h2>

      <section class="auction-detail">
        <h3>{{ auction.title }}</h3>
        <p><strong>開催日:</strong> {{ auction.date }}</p>
        <p><strong>締切日:</strong> {{ auction.deadline }}</p>
        <p><strong>概要:</strong> {{ auction.description }}</p>
      </section>

      <!-- 入札入力表 -->
      <section class="bid-table-section">
        <h3>入札アイテム一覧</h3>
        <table class="bid-table">
          <thead>
            <tr>
              <th>ID</th>
              <th>アイテム名</th>
              <th>説明</th>
              <th>入札金額（円）</th>
              <th>備考</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="item in auction.items" :key="item.id">
              <td>{{ item.id }}</td>
              <td>{{ item.name }}</td>
              <td>{{ item.description }}</td>
              <td>
                <input type="number" v-model="item.bidPrice" class="bid-input" />
              </td>
              <td>
                <input type="text" v-model="item.memo" class="memo-input" />
              </td>
            </tr>
          </tbody>
        </table>
        <p v-if="attention" class="attention">{{ attention }}</p>
<button
  class="submit-button"
  :disabled="!isSubmittable"
  :class="{ disabled: !isSubmittable }"
  @click="handleSubmit"
>
  まとめて送信
</button>
      </section>

      <!-- 確認モーダル -->
      <div class="modal" v-if="showModal">
        <div class="modal-content">
          <h3>以下の内容で入札しますか？</h3>
          <table class="bid-table">
            <thead>
              <tr>
                <th>ID</th>
                <th>アイテム名</th>
                <th>入札金額（円）</th>
                <th>備考</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="item in submittedBids" :key="item.id">
                <td>{{ item.id }}</td>
                <td>{{ item.name }}</td>
                <td>{{ item.bidPrice }}</td>
                <td>{{ item.memo }}</td>
              </tr>
            </tbody>
          </table>
          <div class="modal-buttons">
            <button @click="confirmSubmit" class="confirm-button">確定</button>
            <button @click="closeModal" class="cancel-button">入力し直す</button>
          </div>
        </div>
      </div>

<!-- 完了モーダル -->
<div class="modal" v-if="showComplete">
  <div class="modal-content">
    <h3>入札送信が完了しました。</h3>
    <div class="modal-buttons">
      <NuxtLink to="/customer/" class="button-link">他の入札会を見にいく</NuxtLink>
      <button @click="editAgain" class="edit-button">今の入札状況を編集する</button>
    </div>
  </div>
</div>


    </main>
    <Footer />
  </div>
</template>

<script setup>
import { reactive, ref } from 'vue'
import { useRoute } from 'vue-router'
import Header from '~/components/Header.vue'
import Footer from '~/components/Footer.vue'

const route = useRoute()
const id = route.params.id

const showModal = ref(false)
const showComplete = ref(false)
const submittedBids = ref([])

const auction = reactive({
  id,
  title: '2025年6月度 電子機器入札会',
  date: '2025-06-10',
  deadline: '2025-06-08',
  description: '法人・個人向けの電子機器入札会です。',
  items: [
    { id: 1, name: '業務用プリンター', description: '高速プリンター', bidPrice: '', memo: '' },
    { id: 2, name: 'プロジェクター', description: '会議室用', bidPrice: '', memo: '' },
    { id: 3, name: 'IP電話機', description: 'クラウド対応電話', bidPrice: '', memo: '' }
  ]
})

function openModal() {
  submittedBids.value = auction.items.filter(i => i.bidPrice !== '' && Number(i.bidPrice) > 0)
  if (submittedBids.value.length > 0) {
    showModal.value = true
  }
}

function closeModal() {
  showModal.value = false
}

function confirmSubmit() {
  showModal.value = false
  showComplete.value = true
  // ★ ここでAPI送信処理を入れることも可
}

function closeComplete() {
  showComplete.value = false
}

const isSubmittable = computed(() =>
  auction.items.some(item => item.bidPrice !== '' && Number(item.bidPrice) > 0)
)

const attention = ref('')

function handleSubmit() {
  const valid = auction.items.some(item => item.bidPrice !== '' && Number(item.bidPrice) > 0)
  if (!valid) {
    attention.value = '最低でも1つは入札金額を入力してください。'
    return
  }
  submittedBids.value = auction.items.filter(item => item.bidPrice !== '' && Number(item.bidPrice) > 0)
  attention.value = ''
  showModal.value = true
}

function editAgain() {
  showComplete.value = false
}



</script>

<style scoped lang="scss">
.main {
  padding: 40px;
  max-width: 1000px;
  margin: auto;
}
.title {
  text-align: center;
  margin-bottom: 30px;
}
.auction-detail {
  margin-bottom: 40px;
  background: #f9f9f9;
  padding: 20px;
  border-radius: 8px;
}
.bid-table {
  width: 100%;
  border-collapse: collapse;
  background: #fff;
  margin-bottom: 20px;
}
.bid-table th,
.bid-table td {
  padding: 12px;
  border: 1px solid #ccc;
  text-align: left;
}
.bid-input,
.memo-input {
  width: 100%;
  padding: 6px 10px;
  box-sizing: border-box;
  border: 1px solid #999;
  border-radius: 4px;
}
.submit-button {
  padding: 10px 20px;
  background-color: #28a745;
  color: #fff;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}
.modal {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 999;
}
.modal-content {
  background: #fff;
  padding: 30px;
  border-radius: 8px;
  width: 90%;
  max-width: 600px;
}
.modal-buttons {
  margin-top: 20px;
  text-align: right;
}
.confirm-button,
.cancel-button,
.close-button {
  padding: 8px 16px;
  margin-left: 10px;
  border-radius: 4px;
  cursor: pointer;
}
.confirm-button {
  background-color: #007bff;
  color: #fff;
}
.cancel-button {
  background-color: #ccc;
}
.close-button {
  background-color: #28a745;
  color: #fff;
  float: right;
}


.button-link {
  display: inline-block;
  padding: 10px 16px;
  margin: 10px 5px 0 0;
  border: none;
  border-radius: 4px;
  text-decoration: none;
  font-size: 14px;
  text-align: center;
  line-height: 1;
  box-sizing: border-box;
  cursor: pointer;
  background-color: #007bff;
  color: white;
}

.edit-button {
  display: inline-block;
  padding: 10px 16px;
  margin: 10px 5px 0 0;
  border: none;
  border-radius: 4px;
  text-decoration: none;
  font-size: 14px;
  text-align: center;
  line-height: 1;
  box-sizing: border-box;
  cursor: pointer;
  background-color: #6c757d;
  color: white;
}
.submit-button.disabled {
  background-color: #ccc;
  cursor: not-allowed;
}

.attention {
  color: red;
  margin-top: 10px;
  font-weight: bold;
}

</style>
