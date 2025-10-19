<script setup>
import { ref } from "vue";
import closeIcon from "../assets/close.svg";
import checkIcon from "../assets/check.svg";

const statusImages = {
  close: closeIcon,
  check: checkIcon,
}

const STATUS = {
  PENDING: null,
  REJECTED: false,
  COMPLETED: true,
};

const cardIndex = ref("01");
const flipText = ref('Перевернуть');
const word = ref('Card');
const statusImage = ref(null);
const isFlipped = ref(false);
const status = ref(STATUS.PENDING);
const emit = defineEmits(['flip-card', 'check-status'])


function flipCard() {
  isFlipped.value = true;
  word.value = 'Карточка'
  emit('flip-card');
}


function setStatus(newStatus) {
  status.value = newStatus;
  statusImage.value = newStatus ? statusImages.check : statusImages.close;
  isFlipped.value = false;
  flipText.value = 'завершено';
  emit('check-status', status.value);
}

</script>


<template>
  <div class="card" @click.once="flipCard">
    <div class="card__header">
      <p class="card__header-number">{{ cardIndex }}</p>
      <Transition name="fade">
        <img v-if="status !== STATUS.PENDING" key="icon" class="card__header-image" :src="statusImage"
          :alt="status === STATUS.COMPLETED ? 'Правильно' : 'Не правильно'" />
      </Transition>
    </div>
    <Transition name="fade" mode="out-in">
      <p class="card__word" :key="word">{{ word }}</p>
    </Transition>
    <div class="card__flip">
      <Transition name="fade" mode="out-in">
        <p v-if="!isFlipped" key="text">{{ flipText }}</p>
        <div v-else key="buttons" class="card__flip-buttons">
          <button class="card__button" type="button" @click.stop="setStatus(STATUS.REJECTED)">
            <img class="card__button-image" :src="statusImages.close" alt="Нет" />
          </button>
          <button class="card__button" type="button" @click.stop="setStatus(STATUS.COMPLETED)">
            <img class="card__button-image" :src="statusImages.check" alt="Да" />
          </button>
        </div>
      </Transition>
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
  gap: 133px;
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

.card__header {
  display: flex;
  align-self: flex-start;
  gap: 50px;
  align-items: center;
  padding-block-start: 4px;
  margin-inline-start: 35px;
  font-weight: 400;
  font-size: 14px;
  z-index: 1;
  min-height: 51px;
}

.card__header-number {
  background-color: var(--color-primary-inverted);
}

.card__header-image {
  width: 39px;
  height: 39px;
  margin: 4px;
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
  margin-block-end: 14px;
  min-height: 24px;
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

.card__button {
  display: flex;
  width: 24px;
  height: 100%;
  align-items: center;
  justify-content: center;
  background-color: transparent;
  border: none;
  cursor: pointer;
  padding: 0;
}

.card__button-image {
  width: 20px;
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.25s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}

.card__flip-buttons {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 32px;
  padding-inline: 8px;
}
</style>