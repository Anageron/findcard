<script setup>
defineProps({
  healthCount: {
    type: Number,
    required: true
  }
})
</script>

<template>
  <div class="header__score">
    <span>{{ healthCount }}</span>
    <img class="icon" src="../assets/heart.svg" alt="" />
  </div>
</template>

<style scoped>
.header__score {
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 12px 24px;
  background-color: var(--color-card-edging);
  border-radius: 36px;
  color: var(--color-bg-button);
}
</style>