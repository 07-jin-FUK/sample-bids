<template>
  <div class="page-wrap">
    <main class="main">
      <h2 class="title">初期設定のお客様はタイプCの入札会に参加することはできます。</h2>



      <div class="sort-buttons">
        <button :class="{ active: currentFilter === 'all' }" @click="setFilter('all')">全て見る</button>
        <button :class="{ active: currentFilter === '電気計測器' }" @click="setFilter('電気計測器')">電気計測器</button>
        <button :class="{ active: currentFilter === '分析機器' }" @click="setFilter('分析機器')">分析機器</button>
        <button :class="{ active: currentFilter === 'OAその他' }" @click="setFilter('OAその他')">OAその他</button>
      </div>
      
           <button
  class="submit-button"
  @click="submitChecked"
  v-if="isAnyChecked"
>チェックした入札会へ登録する</button>

      <table class="bids-table">
<thead>
  <tr>
<th>
  <span class="checkmark-indicator">✓</span>
</th>
    <th>案件番号</th>
    <th>カテゴリー</th>
    <th>入札会名称</th>
    <th>入札開始</th>
    <th>入札終了</th>
    <th>詳細</th>
  </tr>
</thead>

<tbody>
  <tr v-for="bid in filteredBids" :key="bid.id">
<td class="checkbox-cell">
  <label class="checkbox-label">
    <input type="checkbox" v-model="bid.checked" />
    <span class="checkmark">✓</span>
  </label>
</td>
    <td>{{ bid.projectId }}</td>
    <td>{{ bid.category }}</td>
    <td>{{ bid.name }}</td>
    <td>{{ formatStartDate(bid.date) }}</td>
    <td>{{ formatEndDate(bid.date) }}</td>
    <td>
              <div v-if="bid.status === '詳細'">
                <NuxtLink :to="`/customer/auction/${bid.id}`" class="join-button">詳細</NuxtLink>
              </div>
              <div class="else-type" v-else>
                {{ bid.status }}
              </div>
            </td>
          </tr>
        </tbody>
      </table>
    </main>
  </div>
</template>

<script setup>
import Header from "~/components/Header.vue";
import Footer from "~/components/Footer.vue";

import { ref, computed } from "vue";

const currentFilter = ref("all");

const setFilter = (category) => {
  currentFilter.value = category;
};

const mockBids = ref([
  {
    id: 101,
        projectId: '07334',
    category: "電気計測器",
    type: "複数品",
    name: "2504入札会",
    date: "2025年07月10日 09:00～ 2025年07月30日 17:00",
    status: "準備中",
  },
  {
    id: 102,
        projectId: '07332',
    category: "OAその他",
    type: "単品",
    name: "12台 ICP発行分析装置 ICP-OESシステム(VDV仕様)5100 Agilent",
    date: "2025年06月10日 09:00～ 2025年06月30日 17:00",
    status: "詳細",
  },
  {
    id: 103,
        projectId: '07330',
    category: "分析機器",
    name: "分析系入札会",
    date: "2025年04月17日 09:00～ 2025年04月30日 17:00",
    status: "詳細",
  },
]);

const filteredBids = computed(() => {
  if (currentFilter.value === "all") return mockBids.value;
  return mockBids.value.filter((bid) => bid.category === currentFilter.value);
});

const formatStartDate = (fullDate) => {
  return fullDate.split('～')[0].trim()
}

const formatEndDate = (fullDate) => {
  return fullDate.split('～')[1]?.trim() || ''
}

const isAnyChecked = computed(() => {
  return mockBids.value.some((bid) => bid.checked)
})

const submitChecked = async () => {
  const selected = mockBids.value.filter((bid) => bid.checked)

  if (selected.length === 0) {
    alert('1件以上選択してください')
    return
  }

  const payload = selected.map(bid => ({ projectId: bid.projectId }))

  try {
    await $fetch('/api/customer/check', {
      method: 'POST',
      body: payload,
    })

    alert('送信が完了しました')
  } catch (e) {
    console.error(e)
    alert('送信エラーが発生しました')
  }
}

</script>

<style scoped lang="scss">
.main {
  flex: 1;
  margin: 0 auto;
  padding: 0 30px 0 60px;
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
  border-collapse: collapse;
  background: #fff;
  margin-left: 120px;
}

.bids-table th,
.bids-table td {
  border: 0.3px solid #707070;
  text-align: left;
  vertical-align: middle;
  line-height: 1;
  padding: 10px 15px ;
    font-size: 0.875em;
}

.bids-table td {
color: #1F1F1F;
}

.bids-table th {
  background-color: #000000;
  color: #ffffff;
}

.bids-table tr:hover {
  background-color: #f0f8ff;
}

.join-button {
background-color: #000000;
padding: 5px 7px ;
color: #ffffff;
border-radius: 7px;
font-size: 0.857em;
}

.else-type{
    font-size: 0.857em;
}

.back-to-mypage {
  display: inline-block;
  margin-bottom: 20px;
  color: #007bff;
  text-decoration: none;
  font-weight: bold;
}
.back-to-mypage:hover {
  text-decoration: underline;
}

.sort-buttons {
  display: flex;
  gap: 15px;
  margin-bottom: 20px;
    margin-left: 120px;


  button {
    padding: 10px 15px;
    background: #ffffff;
    border: 1px solid #000000;
    border-radius: 20px;
    cursor: pointer;
    font-size: 0.875em;
    line-height: 1;

    &.active {
      background: #e88600;
      color: #fff;
      font-weight: bold;
      border: unset;
    }
  }
}

.checkmark-indicator{
    font-weight: bold;
}

.checkbox-cell {
  padding: 0; // ラベル内にパディングを移動するため
}

.checkbox-label {
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  width: 100%;
  height: 100%;

  input[type='checkbox'] {
    opacity: 0;
    width: 0;
    height: 0;
    position: absolute;
  }

  .checkmark {
    font-size: 1em;
    font-weight: bold;
    color: #1F1F1F;
    opacity: 0;
    transition: opacity 0.2s ease;
  }

  input[type='checkbox']:checked + .checkmark {
    opacity: 1;
  }
}

.submit-button {
  margin-bottom: 10px;
  padding: 10px 20px;
  background: #000;
  color: #fff;
  font-size: 0.875em;
  cursor: pointer;
      margin-left: 120px;


  &:hover {
    opacity: 0.8;
  }
}





</style>
