<script setup>
import { ref } from "vue";
import Button from "./components/Button.vue";
import Score from "./components/Score.vue";
import Card from "./components/Card.vue";

const hpCount = ref(100);
const cardInfo = ref({
  word: 'Card',
  translation: 'Карточка',
  state: 'closed',
  status: 'pending'
})


</script>

<template>
  <header class="header">
    Запомни слово
    <Score :hp-count="hpCount"/>
  </header>
  <main class="main"> 
    <Card 
      v-bind="cardInfo"
      @update:state="value => cardInfo.state = value"
      @update:status="value => cardInfo.status = value"
    />
    <Button>Начать игру</Button>
  </main> 
</template>

<style scoped>

.header{
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

.main{
  display: flex;
  flex-direction: column;
  gap:100px;
  justify-content: center;
  align-items: center;
  height: 100vh;
}
</style>
