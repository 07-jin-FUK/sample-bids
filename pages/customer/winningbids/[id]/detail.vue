<template>
  <div class="page-wrap">
    <Header />
    <Sidebar />
    <main class="main">
      <h2 class="title">落札アイテム詳細（ID: {{ id }}）</h2>

      <section v-if="item">
        <table class="detail-table">
            <tbody>
          <tr>
            <th>案件番号</th>
            <td>{{ item.caseNo }}</td>
            <th>No.</th>
            <td>{{ String(item.no).padStart(4, "0") }}</td>
            <th>リスト番号</th>
            <td>{{ item.listNo }}</td>
          </tr>
          <tr>
            <th>商品名</th>
            <td colspan="5">{{ item.name }}</td>
          </tr>
          <tr>
            <th>型番</th>
            <td colspan="5">{{ item.model }}</td>
          </tr>
          <tr>
            <th>メーカー名</th>
            <td colspan="5">{{ item.maker }}</td>
          </tr>
          <tr>
            <th>購入年月日</th>
            <td colspan="1">{{ item.purchaseDate }}</td>
            <th>製造番号</th>
            <td colspan="4">{{ item.serialNo }}</td>
          </tr>
    
          <tr>
            <th>付属品</th>
            <td colspan="5">{{ item.accessories }}</td>
          </tr>
          <tr class="high-row">
            <th>備考</th>
            <td colspan="5">{{ item.memo }}</td>
          </tr>
          <tr>
            <th>仮落札金額</th>
            <td colspan="5">{{ item.price.toLocaleString() }} 円</td>
          </tr>
          <tr><th class="blanc-sell"></th></tr>
          <tr>
            <th>提示金額</th>
            <td colspan="5">{{ (item.suggestedPrice ?? item.price).toLocaleString() }} 円</td>
          </tr>
          <tr>
            <th>管理者メッセージ</th>
            <td colspan="5">{{ item.adminMessage || "未入力" }}</td>
          </tr>
          </tbody>
        </table>

        <div class="response-section">
          <label>対応選択：</label>
          <select v-model="item.userResponse">
            <option value="承諾">承諾</option>
            <option value="キャンセル">キャンセル</option>
            <option value="交渉">交渉</option>
          </select>
        </div>
        
<section v-if="item.userResponse === '交渉'" class="logs-wrap">
  <h3 class="logs-title">交渉履歴</h3>
  <ul class="logs-list" ref="logsContainer">
    <li
      v-for="log in [...logs]"
      :key="log.timestamp"
      :class="['log-item', log.sender]"
    >
      <span class="time">{{ formatTime(log.timestamp) }}</span>
      <p class="text">{{ log.text }}</p>
    </li>
  </ul>
</section>


        <div v-if="item.userResponse === '交渉'" class="negotiation-box">
          <input v-model="item.negotiationPrice" type="text" placeholder="希望価格（円）" />
          <textarea v-model="item.negotiationMemo" placeholder="交渉理由などを記入"></textarea>
        </div>

        <div class="submit-wrap">
          <button class="submit-button" @click="submit">この内容で送信</button>
        </div>
      </section>
    </main>

  </div>
</template>

<script setup lang="ts">

import { useRoute } from "vue-router";
import { ref, reactive } from "vue";
import Header from "@/components/Header.vue";
import Sidebar from "@/components/Sidebar.vue";
import Footer from "@/components/Footer.vue";


import { onMounted, watch, nextTick } from 'vue'


/* ---------- 型 ---------- */
interface Message {
  sender: 'you' | 'admin'
  text: string
  timestamp: number
}
/* ---------- ルートパラメータ ---------- */
const route = useRoute()
const id = route.params.id as string

// 仮データ（将来APIから取得予定）
const item = reactive({
  id,
  caseNo: "07334",
  no: 1,
  listNo: "1",
  projectId: "07334",
  name: "DTV MULTI SIGNAL GENERATOR",
  model: "MSR3100A",
  maker: "営電",
  purchaseDate: "2021-06-10",
  serialNo: "SN123456789",
  accessories: "ケーブル",
  memo: "多少の傷あり",
  qty: 10,
  price: 850000,
  suggestedPrice: "",
  adminMessage: "",
  userResponse: "承諾",
  negotiationPrice: "",
  negotiationMemo: "",
});

const showModal = ref(false);


const logs = reactive<Message[]>([])

/* ---------- 送信処理 ---------- */
function submit() {
  const payload = {
    id: item.id,
    response: item.userResponse,
    negotiationPrice:
      item.userResponse === '交渉' ? item.negotiationPrice : null,
    negotiationMemo:
      item.userResponse === '交渉' ? item.negotiationMemo : null,
  }

  // 1) 自分の送信をログに追加
  addLog(
    'you',
    `${payload.response}${
      payload.response === '交渉'
        ? ` / 希望金額: ${payload.negotiationPrice} / 内容: ${payload.negotiationMemo}`
        : ''
    }`
  )

  // 2) API 送信（ここはダミーで console）
  console.log('送信内容:', payload)

  // 3) モーダル表示
  showModal.value = true

  // 4) デモ用：2 秒後に管理者から返信が来た体でログ追記
  fakeAdminReply()
}

/* ---------- ログ操作 ---------- */
function addLog(sender: Message['sender'], text: string) {
  logs.push({ sender, text, timestamp: Date.now() })
}

function fakeAdminReply() {
  setTimeout(() => {
    addLog(
      'admin',
      item.userResponse === '交渉'
        ? '交渉金額を確認しました。再検討します。'
        : '承諾を受け付けました。'
    )
  }, 2000)
}

/* ---------- 共通 util ---------- */
function formatTime(ts: number) {
  const date = new Date(ts)
  const month = String(date.getMonth() + 1).padStart(2, '0')
  const day = String(date.getDate()).padStart(2, '0')
  return `${month}月${day}日`
}


const logsContainer = ref<HTMLElement | null>(null)

watch(
  () => logs.length,
  async () => {
    await nextTick()
    if (logsContainer.value) {
      logsContainer.value.scrollTop = logsContainer.value.scrollHeight
    }
  }
)

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
  margin: 30px 0;
  text-align: left;
  font-weight: normal;
  font-size: 0.875em;
  position: relative;
}
.detail-table {
  margin: 0 auto;
  border-collapse: collapse;
  th,
  td {
    border: 0.3px solid #707070;
    text-align: left;
    vertical-align: middle;
    line-height: 1;
    font-size: 0.875em;
    height: 55px;
    padding: 0 10px 0 10px;
  }

  th {
    background-color: #000000;
    color: #ffffff;
  }
  td {
    min-width: 150px;
  }
}
.response-section {
  margin: 20px auto;
}
.negotiation-box {
  margin-bottom: 20px;
  input,
  textarea {
    width: 100%;
    padding: 8px;
    margin-top: 5px;
    border: 1px solid #aaa;
    border-radius: 4px;
  }
  textarea {
    height: 40px;
    resize: vertical;
  }
}
.submit-wrap {
  text-align: center;
}
.submit-button {
  background: #000;
  color: #fff;
  padding: 10px 25px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}
.submit-button:hover {
  background: #333;
}
.modal {
  margin-top: 20px;
  padding: 20px;
  border: 1px solid #0a0;
  background: #e0ffe0;
  text-align: center;
}

/* ↓送受信ログ */
.logs-wrap {
  margin: 30px auto 20px;
  max-width: 960px;
    width: 100%;
    background-color: #cccbcb;
padding:20px;
border-radius: 8px;
}
.logs-title {
  font-size: 0.9rem;
  margin-bottom: 10px;
}
.logs-list {
  list-style: none;
  padding: 0;
  width: 100%;
  display: flex;
  flex-direction: column;
height: 200px; 
  overflow-y: auto;
}
.log-item {
  display: flex;
    flex-direction: column; // ←ここを追加（縦並びに）

  align-items: flex-start;
  gap: 8px;
  margin-bottom: 8px;

  .time {
    font-size: 0.75rem;
    color: #888;
    min-width: 50px;
    text-align: end;
  }
  .text {
    background: #f5f5f5;
    border-radius: 6px;
    padding: 6px 10px;
    font-size: 0.85rem;
    line-height: 1.45;
  }

  &.you {
    width: 100%;
        flex-direction: row; 
    justify-content: flex-start;
    .text {
      background: #dff0ff;
    }
  } 
  
  &.admin{
    width: 100%;
        flex-direction: row; 
    justify-content: flex-end;
    .text {
      background: #eafbe5;
    }
  } 
}

.blanc-sell{
    background: none !important;
    border: none !important;
    height:35px !important;
}
</style>
