<script setup>
import { nextTick, onMounted, ref } from "vue";
import Button from "./components/Button.vue";
import Score from "./components/Score.vue";
import Card from "./components/Card.vue";

const healthCount = ref(100);
const cards = ref([]);
const gameKey = ref(0);

onMounted(()=>getData() )

async function getData() {
  gameKey.value += 1;
  try {
    const res = await fetch('http://localhost:8080/api/random-words');
    if (!res.ok) throw new Error('Не удалось загрузить слова');
    const rawData = await res.json();
    
    cards.value = (Array.isArray(rawData) ? rawData : []).map(item => ({
      word: item.word || '',
      translation: item.translation || '',
      state: 'closed',   
      status: 'pending' 
    }));
  } catch (err) {
    console.error(err);
    cards.value = [];
  }

}
function updateCardState(index, newState) {
  if (cards.value[index]) {
    cards.value[index].state = newState;
  }
  
}

function updateCardStatus(index, newStatus) {
  if (cards.value[index]) {
    cards.value[index].status = newStatus;
    healthCount.value = newStatus === "fail"
      ? Math.max(0, healthCount.value - 10)
      : Math.min(1000, healthCount.value + 4);
  }
}


</script>

<template>
  <header class="header">
    Запомни слово
    <Score :health-count="healthCount" />
  </header>
  <main class="main">
    <section class="cards">
      <Card 
        v-for="(card, index) in cards" 
        :key="`${gameKey}-${index}`"
        v-bind="card" 
        :index="index"
        @update:state="value => updateCardState(index, value)"
        @update:status="value => updateCardStatus(index, value)" />
    </section>
    <Button @click="getData()" >Начать игру</Button>
  </main>
</template>

<style scoped>
.header {
  display: flex;
  justify-content: space-between;
  padding: 36px 62px;
  color: var(--color-text-card-action);
  font-family: var(--font-family);
  font-weight: 700;
  font-size: 16px;
  line-height: 150%;
  letter-spacing: 0.12em;
}

.main {
  padding-block-start: 50px;
  display: flex;
  flex-direction: column;
  gap: 100px;
  justify-content: center;
  align-items: center;

}

.cards {
  display: flex;

  gap: 100px;
  flex-wrap: wrap;

}
</style>
