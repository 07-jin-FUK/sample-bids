<template>
  <div class="page-wrap">
    <Header />
      <Sidebar />
    <main class="main">
      <h2 class="title">落札案件詳細（ID: {{ bidId }}）</h2>

      <template v-if="bid">
        <h3>{{ bid.title }}</h3>
        <p>落札日: {{ bid.date }}</p>

        <table class="detail-table">
          <thead>
            <tr>
              <th>ID</th>
              <th>アイテム名</th>
              <th>落札金額（円）</th>
              <th>ステータス</th>
              <th>管理者メッセージ</th>
              <th>提示金額</th>
              <th>対応選択</th>
            </tr>
          </thead>
          <tbody>
            <tr
              v-for="item in bid.items"
              :key="item.id"
                :class="{
                'responded': item.status === '返信済み',
                'cancelled': item.status === '失注'
              }"
            >
              <td>{{ item.id }}</td>
              <td>{{ item.name }}</td>
              <td>{{ item.price.toLocaleString() }}</td>
              <td>{{ item.status }}</td>
              <td>{{ item.adminMessage }}</td>
              <td>{{ item.suggestedPrice ? item.suggestedPrice.toLocaleString() + ' ' : '-' }}</td>
              <td>
                <select
                  v-model="item.userResponse"
                  :disabled="item.status === '失注'"
                >
                  <option disabled value="">選択してください</option>
                  <option value="承諾">承諾</option>
                  <option value="キャンセル">キャンセル</option>
                  <option value="交渉">交渉</option>
                </select>
              </td>
            </tr>
          </tbody>
        </table>

        <div class="submit-button-wrap">
          <button class="submit-button" @click="submitResponses">送信</button>
        </div>
        
        <!-- モーダル -->
<div v-if="showModal" class="modal-overlay">
  <div class="modal-content">
    <p>データを送信しました。返事をお待ちください。</p>
    <button  class="submit-button" @click="showModal = false">OK</button>
  </div>
</div>

      </template>

      <p v-else>該当する落札案件が見つかりません。</p>
    </main>
    <Footer />
  </div>
</template>

<script setup>
import { useRoute } from 'vue-router'
import { computed } from 'vue'
import Header from '~/components/Header.vue'
import Footer from '~/components/Footer.vue'

const route = useRoute()
const bidId = route.params.id

const bidDetails = [
  {
    id: '101',
    title: '2025年度 電子機器調達',
    date: '2025-04-10',
    items: [
      {
        id: 1,
        name: 'ノートPC',
        price: 850000,
        status: '入荷待ち',
        adminMessage: '7月上旬予定です',
        suggestedPrice: null
      },
      {
        id: 2,
        name: 'プリンター',
        price: 950000,
        status: '状態確認後管理者より返信あり',
        adminMessage: 'ボディに少々の傷あり',
        suggestedPrice: 900000
      },
      {
        id: 3,
        name: 'モニター',
        price: 650000,
        status: '返信済み',
        adminMessage: 'ボディに少々の傷あり',
        suggestedPrice: 600000
      },
      {
        id: 4,
        name: 'スキャナー',
        price: 480000,
        status: '失注',
        adminMessage: '在庫不足のため入荷できませんでした',
        suggestedPrice: null
      },
      {
        id: 5,
        name: 'Webカメラ',
        price: 150000,
        status: '問題なし',
        adminMessage: '特に問題ありません。',
        suggestedPrice: 150000
      },
      {
        id: 6,
        name: 'ヘッドセット',
        price: 120000,
        status: '完了',
        adminMessage: '納品済',
        suggestedPrice: 120000
      },
      {
        id: 7,
        name: 'マイク',
        price: 120000,
        status: '状態確認後管理者より返信あり',
        adminMessage: 'スタンド、ボイスチェンジャーありのため提示金額UP',
        suggestedPrice: 150000
      }
    ]
  },
  {
    id: '102',
    title: '社内ネットワーク更新工事',
    date: '2025-03-28',
    items: [
      {
        id: 1,
        name: 'ルーター',
        price: 550000,
        status: '返信済み',
        adminMessage: '承認済みです',
        suggestedPrice: 540000
      },
      {
        id: 2,
        name: 'スイッチ',
        price: 400000,
        status: 'キャンセル',
        adminMessage: '予算超過のためキャンセル',
        suggestedPrice: null
      }
    ]
  },
  {
    id: '103',
    title: '備品購入（PC周辺機器）',
    date: '2025-02-15',
    items: [
      {
        id: 1,
        name: 'キーボード',
        price: 100000,
        status: '完了',
        adminMessage: '納品済です',
        suggestedPrice: 100000
      },
      {
        id: 2,
        name: 'マウス',
        price: 120000,
        status: '完了',
        adminMessage: '納品済です',
        suggestedPrice: 120000
      }
    ]
  }
]

const bid = computed(() => bidDetails.find(b => b.id === bidId))

import { ref } from 'vue'

const showModal = ref(false)

function submitResponses() {
  console.log('送信された対応内容:', bid.value?.items.map(item => ({
    id: item.id,
    name: item.name,
    userResponse: item.userResponse || '未選択'
  })))

  showModal.value = true
}

</script>

<style scoped>
.page-wrap {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
}

.main {
  flex: 1;
  padding: 40px;
  max-width: 1200px;
  margin: auto;
}

.title {
  text-align: center;
  margin-bottom: 20px;
}

.detail-table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 20px;
  background: #fff;
}

.detail-table th,
.detail-table td {
  padding: 10px;
  border: 1px solid #ccc;
  text-align: center;
}

.detail-table th {
  background-color: #f5f5f5;
}

.responded {
  background-color: #e0f7fa;
}

.cancelled {
  background-color: #eee;
  color: #888;
}

.back-to-list {
  display: block;
  margin-bottom: 20px;
  color: #007bff;
  text-decoration: underline;
}

.submit-button-wrap {
  display: flex;
  justify-content: flex-end;
  margin-top: 30px;
}

.submit-button {
  padding: 10px 20px;
  background-color: #007bff;
  color: white;
  border: none;
  cursor: pointer;
  border-radius: 4px;
  margin-top: 20px;
}

.submit-button:hover {
  background-color: #0056b3;
}

.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.4);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1000;
}

.modal-content {
  background: white;
  padding: 30px 40px;
  border-radius: 8px;
  text-align: center;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.3);
}

</style>
