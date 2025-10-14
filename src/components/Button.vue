<script setup>

</script>

<template>
  <button class="button">
    <slot/>
  </button>
</template>

<style scoped>
  .button {
    width: 335px;
    padding: 16px 10px;
    border: none;
    border-radius: 100px;
    color: var(--color-primary-inverted);
    background-color: var(--color-bg-button);
    font-size: 24px;
    font-style: normal;
    font-weight: 400;
    line-height: 36px;
    text-align: center;
    font-family: inherit;

  }

  .button:hover {
    background: var(--color-bg-button-hover);
  }
</style>