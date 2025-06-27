<template>
  <div class="page-wrap">
    <main class="main">
      <h2 class="title">お客様は以下の入札会に参加することはできます。</h2>

      <div class="sort-buttons">
        <button :class="{ active: currentFilter === 'all' }" @click="setFilter('all')">全て見る</button>
        <button :class="{ active: currentFilter === '電気計測器' }" @click="setFilter('電気計測器')">電気計測器</button>
        <button :class="{ active: currentFilter === '分析機器' }" @click="setFilter('分析機器')">分析機器</button>
        <button :class="{ active: currentFilter === 'OAその他' }" @click="setFilter('OAその他')">OAその他</button>
      </div>

      <table class="bids-table">
        <thead>
          <tr>
            <th><span class="checkmark-indicator">✓</span></th>
            <th>状態</th>
            <th>落札方法</th>
            <th>案件番号</th>
            <th>カテゴリー</th>
            <th>入札会名称</th>
            <th>入札開始</th>
            <th>入札終了</th>
            <th>詳細</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="auction in filteredAuctions" :key="auction.id">
            <td class="checkbox-cell">
              <label class="checkbox-label">
                <input type="checkbox" v-model="auction.checked" />
                <span class="checkmark">✓</span>
              </label>
            </td>
            <td>{{ auction.status }}</td>
            <td>{{ auction.bid_method }}</td>
            <td>{{ auction.project_number }}</td>
            <td>{{ auction.category_name }}</td>
            <td>{{ auction.auction_name }}</td>
            <td>{{ auction.start_at }}</td>
            <td>{{ auction.end_at }}</td>
            <td>
              <div v-if="auction.status === '開催中'">
                <NuxtLink :to="`/customer/auction/${auction.id}`" class="join-button">詳細</NuxtLink>
              </div>
              <div class="else-type" v-else>
                {{ auction.status }}
              </div>
            </td>
          </tr>
        </tbody>
      </table>
    </main>
  </div>
</template>

<script setup>
import { ref, computed, watch } from 'vue';

const currentFilter = ref('all');
const setFilter = (category) => {
  currentFilter.value = category;
};

const auctions = ref([
  {
    id: 101,
    status: '準備中',
    bid_method: '一括落札',
    project_number: '07334',
    category_name: '電気計測器',
    auction_name: '2504入札会',
    start_at: '2025年07月10日 09:00',
    end_at: '2025年07月30日 17:00',
  },
  {
    id: 102,
    status: '開催中',
    bid_method: '個別落札',
    project_number: '07332',
    category_name: 'OAその他',
    auction_name: 'ICP-OESシステム 12台',
    start_at: '2025年06月10日 09:00',
    end_at: '2025年06月30日 17:00',
  },
  {
    id: 103,
    status: '開催中',
    bid_method: '個別落札',
    project_number: '07330',
    category_name: '分析機器',
    auction_name: '分析系入札会',
    start_at: '2025年04月17日 09:00',
    end_at: '2025年04月30日 17:00',
  },
]);

const filteredAuctions = computed(() => {
  if (currentFilter.value === 'all') return auctions.value;
  return auctions.value.filter((auction) => auction.category_name === currentFilter.value);
});

// チェック状態監視（モックログ用）
watch(
  () => auctions.value.map((auction) => auction.checked),
  (newVals, oldVals) => {
    newVals.forEach((newVal, idx) => {
      if (newVal !== oldVals[idx]) {
        const a = auctions.value[idx];
        console.log(`【モック送信】${a.project_number} を ${newVal ? '登録' : '解除'}しました`);
      }
    });
  },
  { deep: true }
);
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
</style>
