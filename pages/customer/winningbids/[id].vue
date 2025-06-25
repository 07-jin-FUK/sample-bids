<template>
  <div class="page-wrap">
    <Header />
    <Sidebar />
    <main class="main">
      <h2 class="title">落札案件詳細（ID: {{ id }}）</h2>

      <section>
        <table class="bids-table">
          <thead>
            <tr>
              <th>No.</th>
              <th>案件番号</th>
              <th>商品名</th>
              <th>落札金額（円）</th>
              <th>管理者メッセージ</th>
              <th>提示金額</th>
              <th>対応選択</th>
            </tr>
          </thead>
        <tbody>
  <template v-for="(item, idx) in winningItems" :key="item?.id || idx">
    <tr
      :class="{
        responded: item.userResponse === '承諾',
        cancelled: item.userResponse === 'キャンセル',
      }"
    >
      <td class="text-center">{{ String(idx + 1).padStart(4, '0') }}</td>
      <td>{{ item.projectId }}</td>
      <td>{{ item.name }}</td>
      <td class="text-right">{{ item.price.toLocaleString() }}</td>
      <td>{{ item.adminMessage || '-' }}</td>
      <td class="text-right">{{ (item.suggestedPrice ?? item.price).toLocaleString() }}</td>
      <td>
<select v-model="item.userResponse">
  <option value="承諾">承諾</option>
  <option value="キャンセル">キャンセル</option>
  <option value="交渉">交渉</option>
</select>

      </td>
    </tr>

    <!-- 交渉が選択された場合のみ表示 -->
    <tr v-if="item.userResponse === '交渉'" class="negotiation-row">
      <td colspan="3">再提示金額と価格交渉理由を記載ください</td>
      <td colspan="1">
        <div class="negotiation-fields">
          <input
            type="text"
            v-model="item.negotiationPrice"
            placeholder="希望価格を入力"
          />
        </div>
      </td>
            <td colspan="3">
        <div class="negotiation-fields-details">
    <textarea
          
            v-model="item.negotiationMemo"
            placeholder="備考（交渉理由など）"
          />
        </div>
      </td>
    </tr>
  </template>
</tbody>

        </table>
      </section>

      <section class="submit-section">
        <button @click="submitResponses" class="submit-button">送信</button>
      </section>

      <div v-if="showModal" class="modal-overlay">
        <div class="modal-content">
          <p>データを送信しました。返事をお待ちください。</p>
          <button class="submit-button" @click="showModal = false">OK</button>
        </div>
      </div>
    </main>
    <Footer />
  </div>
</template>

<script setup>
import { reactive, computed, ref } from 'vue'
import { useRoute } from 'vue-router'
import Header from '~/components/Header.vue'
import Footer from '~/components/Footer.vue'
import Sidebar from '~/components/Sidebar.vue'

const route = useRoute()
const id = route.params.id

const auction = reactive({
  id,
  items: [
    {
      id: 1,
      projectId: '07334',
      name: 'DTV MULTI SIGNAL GENERATOR ',
      price: 850000,
      status: 'winning',
      adminMessage: '7月上旬予定です',
      suggestedPrice: null,
      userResponse: '',
      negotiationPrice: null,
      negotiationMemo: ''
    },
    {
      id: 2,
      projectId: '07334',
      name: 'DTV信号発生器',
      price: 950000,
      status: 'winning',
      adminMessage: 'ボディに少々の傷あり',
      suggestedPrice: 900000,
      userResponse: '',
      negotiationPrice: null,
      negotiationMemo: ''
    },
  ]
})

const winningItems = computed(() =>
  auction.items.filter(item => item && item.status === 'winning')
)
const showModal = ref(false)

function submitResponses() {
  const payload = winningItems.value.map(item => ({
    id: item.id,
    response: item.userResponse || '未選択',
    negotiationPrice: item.userResponse === '交渉' ? item.negotiationPrice : null,
    negotiationMemo: item.userResponse === '交渉' ? item.negotiationMemo : null,
  }))
  console.log('送信データ:', payload)
  showModal.value = true
}


auction.items.forEach(item => {
  if (!item.userResponse) item.userResponse = '承諾'
})

const payload = winningItems.value.map(item => ({
  id: item.id,
  response: item.userResponse || '承諾',
  negotiationPrice: (item.userResponse || '承諾') === '交渉' ? item.negotiationPrice : null,
  negotiationMemo: (item.userResponse || '承諾') === '交渉' ? item.negotiationMemo : null,
}))

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
  margin: auto;
}

.title {
  margin-bottom: 30px;
  text-align: left;
  font-size: 0.875em;
}

.bids-table {
  width: 100%;
  border-collapse: collapse;
  background: #fff;
  margin-bottom: 30px;

  th,
  td {
    border: 0.3px solid #707070;
    text-align: left;
    vertical-align: middle;
    line-height: 1;
    padding: 10px 15px;
    font-size: 0.875em;
  }

  th {
    background-color: #000000;
    color: #ffffff;
  }

    .negotiation-row{
        height: 60px;
    }
  
  
  .negotiation-row td {
    background: #f9f9f9;
  }



  .negotiation-fields input {
    padding: 6px 10px;
    border: 1px solid #aaa;
    border-radius: 4px;
    font-size: 0.875em;
    width: 80%;
  }
  
    .negotiation-fields-details textarea {
    padding: 6px 10px;
    border: 1px solid #aaa;
    border-radius: 4px;
    font-size: 0.875em;
    width: 90%;
    height: 50px;
  }
  
  
}

.responded {
  background-color: #e0f7fa;
}

.cancelled {
  background-color: #eee;
  color: #888;
}

.submit-section {
  text-align: center;
  margin-bottom: 30px;
}

.submit-button {
  background-color: #000000;
  color: white;
  padding: 10px 25px;
  font-size: 0.875em;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

.submit-button:hover {
  background-color: #1f1f1f;
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
