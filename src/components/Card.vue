<script setup>
import { ref } from "vue";
const isFlipped = ref(false);
const status = ref(null);
const flipText = ref('Перевернуть');
const word = ref('Карточка');
const emit = defineEmits(['flip-card', 'check-status'])

function flipCard() {
   isFlipped.value = !isFlipped.value;

  if (isFlipped.value) {
    flipText.value = 'Перевернуто';
    emit('flip-card');
    emit('check-status'); 
  } else {
    flipText.value = 'Перевернуть';
  }
}


</script>


<template>
  <div class="card" @click="flipCard">
    <div class="card__number">
      01
    </div>
    <p class="card__word">{{ word }}</p>
    <div class="card__flip">
      {{ flip }}
    </div>
  </div>
</template>

<style scoped>
.card {
  position: relative;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 140px;
  width: 250px;
  border-radius: 16px;
  background-color: var(--color-primary-inverted);
  color: var(--color-primary);
}

.card::before {
  content: '';
  position: absolute;
  inset: 28px 20px;
  border: 1px solid var(--color-card-edging);
  border-radius: 16px;
  pointer-events: none;
  z-index: 0;
}

.card:hover {
  box-shadow: 0 0 16px 0 rgba(0, 0, 0, 0.1);
}

.card__number {
  align-self: flex-start;
  margin-block-start: 20px;
  margin-inline-start: 35px;
  font-weight: 400;
  font-size: 14px;
  z-index: 1;
  background-color: var(--color-primary-inverted);
}

.card__word {
  font-weight: 400;
  font-size: 18px;
  text-transform: lowercase;

}

.card__flip {
  display: flex;
  align-items: center;
  justify-content: center;
  padding-block-end: 20px;
  gap: 32px;
  padding-inline: 8px;
  font-weight: 700;
  font-size: 12px;
  line-height: 150%;
  letter-spacing: 0.12em;
  text-transform: uppercase;
  color: var(--color-text-card-action);
  background-color: var(--color-primary-inverted);
  z-index: 1;
}
</style>  