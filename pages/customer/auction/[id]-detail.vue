<template>
  <div class="page-wrap">
    <Header />
    <Sidebar />
    <main class="main">
<h2 class="title">
  <NuxtLink :to="`/customer/auction/${id}`" class="back-button">
    <span class="icon">←</span>
  </NuxtLink>
  入札アイテム詳細
</h2>

      <table class="item-detail-table">
        <tbody>
          <tr>
            <th>案件番号</th><td>{{ item.caseNo }}</td>
            <th>No.</th><td>{{ String(item.no).padStart(4, '0') }}</td>
            <th>リスト番号</th><td>{{ item.listNo }}</td>
        </tr>
          <tr><th>商品名</th><td colspan="5">{{ item.name }}</td></tr>
          <tr><th>型番</th><td colspan="5">{{ item.model }}</td></tr>
          <tr><th>メーカー名</th><td colspan="5">{{ item.maker }}</td></tr>
          <tr><th>購入年月日</th><td colspan="5">{{ item.purchaseDate }}</td></tr>
          <tr><th>製造番号</th><td colspan="5">{{ item.serialNo }}</td></tr>
          <tr><th>付属品</th><td colspan="5">{{ item.accessories }}</td></tr>
          <tr class="high-row"><th>備考</th><td colspan="5">{{ item.memo }}</td></tr>
          <tr>
            <th>数量</th>
<td >{{ form.qty }}</td> 
           <th>入札額</th>
            <td>
    <input type="text" v-model="form.bidPrice" />
            </td>
            <th>入札額小計</th><td colspan="5">{{ subtotal.toLocaleString() }} 円</td>
          </tr>
          <tr class="high-row">
            <th>入札備考</th>
            <td colspan="5"><input type="text" v-model="form.bidMemo" /></td>
          </tr>
        </tbody>
      </table>
          <section class="submit-section">
      <button class="submit-button" @click="submitBid">この商品を入札する</button>
    </section>
    </main>
  </div>
</template>

<script setup>
import Header from '@/components/Header.vue'
import Sidebar from '@/components/Sidebar.vue'
import { useRoute } from 'vue-router'
import { reactive, computed } from 'vue'


// モック（APIで取得する内容）
const item = reactive({
  caseNo: '07334',
  no: 1,
  listNo: '1',
  name: 'DTV MULTI SIGNAL GENERATOR （BS信号発生器）',
  model: 'MSR3100A',
  maker: '営電',
  purchaseDate: '2021-06-10',
  serialNo: 'SN123456789',
  accessories: 'ケーブル',
  memo: '多少の傷あり',
  qty: 10,
  bidPrice: '',
  bidMemo: ''
})

// 入力可能なフォーム用データ（初期値は item から流用）
const form = reactive({
  qty: item.qty,
  desiredQty: item.desiredQty,
  bidPrice: item.bidPrice,
  bidMemo: item.bidMemo
})

// 計算フィールド
const subtotal = computed(() => form.bidPrice * form.qty)

const submitBid = () => {
  const payload = {
    caseNo: item.caseNo,
    itemNo: item.no,
    bidPrice: form.bidPrice,
    bidMemo: form.bidMemo
  }

  // ここでAPI送信処理を行う（今は確認用ログ）
  console.log('送信データ:', payload)

  alert('入札内容を送信しました。')
}

const route = useRoute()
const id = route.params.id
</script>

<style scoped>
.main {
  flex: 1;
  margin: auto;
}

.title {
  margin: 30px 0;
  text-align: left;
  font-weight: normal;
  font-size: 0.875em;
  position: relative;
}
.item-detail-table {
    margin: 0 auto;
  border-collapse: collapse;
  th, td {
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
  td{
    min-width: 150px;
  }
  
  input {
    width: 80%;
    padding: 6px;
    font-size: 0.9em;
    height: 50%;
  }
  
  tr.high-row {
  height: 110px;
}

}

.back-button {
  display: inline-flex;
  align-items: center;
  gap: 6px;
  color: #333;
  padding: 8px 16px;
  font-size: 0.9em;
  cursor: pointer;
  transition: background-color 0.2s;
  margin-top: unset;


  .icon {
    display: inline-flex;
    justify-content: center;
    align-items: center;
    background-color: #333;
    color: #fff;
    border-radius: 50%;
    width: 24px;
    height: 24px;
    font-weight: bold;
    font-size: 0.875em;
  }
}

.submit-section {
  margin: 40px auto;
  text-align: center;
}

.submit-button {
  background-color: #000000;
  color: white;
  padding: 10px 25px;
  font-size: 0.9em;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.2s;

  &:hover {
    background-color: #1f1f1f;
  }
}

</style>
