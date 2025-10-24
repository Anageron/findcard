<script setup>
import { ref } from "vue";
import Button from "./components/Button.vue";
import Score from "./components/Score.vue";
import Card from "./components/Card.vue";

const healthCount = ref(100);
const cardInfo = ref([{
  word: 'Card',
  translation: 'Карточка',
  state: 'closed',
  status: 'pending'
},
{
  word: 'Word',
  translation: 'Слово',
  state: 'closed',
  status: 'pending'

},
{
  word: 'Car',
  translation: 'Машина',
  state: 'closed',
  status: 'pending'

},
{
  word: 'Door',
  translation: 'Дверь',
  state: 'closed',
  status: 'pending'

}
])


</script>

<template>
  <header class="header">
    Запомни слово
    <Score :health-count="healthCount" />
  </header>
  <main class="main">
    <section class="cards"> 
      <Card 
        v-for="(card, index) in cardInfo" :key="index" v-bind="card" :index="index"
        @update:state="value => cardInfo[index].state = value"
        @update:status="value => cardInfo[index].status = value" />
    </section>
    <Button>Начать игру</Button>
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
