<script setup>
import { ref, computed } from "vue";
import closeIcon from "../assets/close.svg";
import checkIcon from "../assets/check.svg";

const STATUS = {
  PENDING: "pending",
  REJECTED: "fail",
  COMPLETED: "success",
};

const statusImages = {
  [STATUS.REJECTED]: closeIcon,
  [STATUS.COMPLETED]: checkIcon,
}


const props = defineProps({
  word: {
    type: String,
    required: true
  },
  translation: {
    type: String,
    required: true
  },
  state: {
    type: String,
    required: true
  },
  status: {
    type: String,
    required: true
  }
})


const emit = defineEmits(['update:state', 'update:status'])

const displayedWord = computed(() => {
  return props.state === 'opened' ? props.translation : props.word;
});

const flipText = computed(() => {
  return props.state === 'opened' ? 'Закрыть' : 'Перевернуть';
})

const statusImage = computed(() => {
  return statusImages[props.status] || null;
});

const cardIndex = ref("01");



function flipCard() {
  if (props.status !== 'pending' || props.state !== 'closed') return;
  emit('update:state', 'opened');
}


function setStatus(isCorrect) {
  if (props.status !== 'pending' || props.state !== 'opened') return;
  const newStatus = isCorrect ? STATUS.COMPLETED : STATUS.REJECTED;
  emit('update:status', newStatus);
}

</script>


<template>
  <div class="card" @click.once="flipCard">
    <div class="card__header">
      <p class="card__header-number">{{ cardIndex }}</p>
      <Transition name="fade">
        <img 
          v-if="props.status !== STATUS.PENDING" 
          class="card__header-image" :src="statusImage"
          :alt="status === STATUS.COMPLETED ? 'Правильно' : 'Не правильно'" />
      </Transition>
    </div>
    <Transition name="fade" mode="out-in">
      <p :key="displayedWord" class="card__word">{{ displayedWord }}</p>
    </Transition>
    <div class="card__flip">
      <Transition name="fade" mode="out-in">
        <p v-if="state === 'closed' && status === 'pending'">{{ flipText }}</p>
        <div v-else-if="state === 'opened' && status === 'pending'" key="buttons" class="card__flip-buttons">
          <button class="card__button" type="button" @click.stop="setStatus(STATUS.REJECTED)">
            <img class="card__button-image" :src="closeIcon" alt="Нет" />
          </button>
          <button class="card__button" type="button" @click.stop="setStatus(STATUS.COMPLETED)">
            <img class="card__button-image" :src="checkIcon" alt="Да" />
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