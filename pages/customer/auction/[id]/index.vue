<template>
  <div class="page-wrap">
    <Header />
    <Sidebar />
    <main class="main">
      <h2 class="title">入札会詳細（ID: {{ id }}）</h2>
<a
  v-if="item && item.imageUrl"
  :href="item.imageUrl"
  target="_blank"
  rel="noopener noreferrer"
  class="photo-button"
>
  写真を見る
</a>

      <section>
        <table class="bids-table">
          <thead>
            <tr class="bg-black text-white text-sm">
              <th>No.</th>
              <th>リスト番号</th>
              <th>商品番号</th>
              <th>出品名</th>
              <th>最低入札額（円）</th>
              <th>入札額（円）</th>
              <th>数量</th>
              <th>入札備考</th>
              <th>明細へ</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(item, idx) in auction.items" :key="item.id" class="border-b hover:bg-gray-50">
              <td class="text-center">{{ String(idx + 1).padStart(4, "0") }}</td>
              <td>{{ item.listNo }}</td>
              <td>{{ String(item.caseNo).padStart(5, "0") }}-{{ String(idx + 1).padStart(4, "0") }}</td>
              <td>{{ item.maker }}｜{{ item.model }}</td>
              <td class="text-right">{{ item.minPrice.toLocaleString() }}</td>
 <td>
  <input
    type="text"
    v-model="item.bidPrice"
    class="w-full border border-gray-400 rounded px-2 py-1 text-right"
    placeholder="金額を入力"
  />
</td>
              <td class="text-right">{{ item.qty }}</td>
              <td class="py-2 px-3">
                <input type="text" v-model="item.memo" class="w-full border border-gray-400 rounded px-2 py-1" placeholder="備考を入力" />
              </td>
              <td class="py-2 px-3 text-center">
                <NuxtLink :to="`/customer/auction/${id}/detail?item=${item.id}`" class="details-link">明細へ</NuxtLink>
              </td>
            </tr>
          </tbody>
        </table>
      </section>
<section style="margin-top: 30px; margin-left: 120px;">
  <p><strong>小計金額：</strong>{{ totalSubtotal.toLocaleString() }} 円</p>
</section>

      <section style="margin-top: 30px; margin-left: 120px;">
  <button @click="submitBids" class="submit-button">入札を確定する</button>
</section>
    </main>
  </div>
  

</template>

<script setup>
import { reactive } from "vue";
import { useRoute } from "vue-router";
import { computed } from 'vue'

const totalSubtotal = computed(() =>
  auction.items.reduce((sum, item) => {
    const price = Number(item.bidPrice) || 0
    const qty = Number(item.qty) || 0
    return sum + price * qty
  }, 0)
)

const route = useRoute();
const id = route.params.id;

const auction = reactive({
  id,
  items: [
    {
      id: 1,
      listNo: "1",
      caseNo: "07334",
      name: "DTV MULTI SIGNAL GENERATOR （BS信号発生器）",
      maker: "営電",
      model: "MSR3100A",
      minPrice: 0,
      bidPrice: null,
      qty: 10,
      desiredQty: null,
      memo: "",
    },
    {
      id: 2,
      listNo: "2",
      caseNo: "07334",
      name: "スペクトラムアナライザ",
      maker: "アドバンテスト",
      model: "R3267",
      minPrice: 1000,
      bidPrice: null,
      qty: 5,
      desiredQty: null,
      memo: "",
    },
  ],
});

const detailLink = (item) => `/customer/auction/${id}/item/${item.id}`;

const submitBids = () => {
  const payload = auction.items.map(item => ({
    id: item.id,
    bidPrice: item.bidPrice,
    desiredQty: item.desiredQty,
    memo: item.memo,
  }))

  // ここでAPIに送る（仮で表示）
  console.log('送信データ:', payload)

  // 実際のAPI送信は例↓
  // await $fetch('/api/submit-bids', {
  //   method: 'POST',
  //   body: payload,
  // })

  alert('入札を送信しました！')
}




</script>

<style scoped lang="scss">
.main {
  flex: 1;
  margin: auto;
}

.title {
  margin: 85px 0 50px;
  text-align: left;
  font-weight: normal;
  font-size: 0.875em;
  margin-left: 120px;
}

.bids-table {
  width: 100%;
  margin-left: 120px;
  border-collapse: collapse;
  background: #fff;

  th,
  td {
    border: 0.3px solid #707070;
    text-align: left;
    vertical-align: middle;
    line-height: 1;
    padding: 10px 15px;
    font-size: 0.875em;
  }

  td {
    color: #1f1f1f;
  }

  th {
    background-color: #000000;
    color: #ffffff;
  }

  tr:hover {
    background-color: #f0f8ff;
  }

  input[type="number"],
  input[type="text"] {
    width: 100%;
    padding: 6px 10px;
    border: 1px solid #aaa;
    border-radius: 4px;
    font-size: 0.9em;
    box-sizing: border-box;
    max-width: 100px;
  }

  .details-link {
background-color: #000000;
padding: 5px 7px ;
color: #ffffff;
border-radius: 7px;
font-size: 0.857em;
  }
}

.submit-button {
  background-color: #000000;
  color: white;
  padding: 10px 25px;
  font-size: 0.875em;
  border: none;
  border-radius: 5px;
  cursor: pointer;

  &:hover {
    background-color: #1f1f1f;
  }
}

.photo-button {
background-color: #000000;
padding: 5px 7px ;
color: #ffffff;
border-radius: 7px;
font-size: 0.857em;
line-height: 1;
margin-left: 120px;

}

</style>
