<template>
  <div class="conversion__result">
    <h2>Результат:</h2>
    <strong>{{ result }} {{ targetCurrency }}</strong>
  </div>
</template>

<script>
export default {
  props: {
    result: Number,
    targetCurrency: String,
  },
};
</script>

<style scoped lang="scss">
.conversion__result {
  text-align: start;
  & h2 {
    font-weight: 600;
    text-transform: lowercase;
  }
}
</style>
