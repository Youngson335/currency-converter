<template>
  <input
    type="number"
    v-model.number="localAmount"
    @input="onInput"
    placeholder="Введите сумму"
    class="conversion__amount"
  />
</template>

<script>
export default {
  props: {
    amount: {
      type: Number,
      required: true,
    },
  },
  data() {
    return {
      localAmount: this.amount,
    };
  },
  watch: {
    amount(newVal) {
      this.localAmount = newVal;
    },
  },
  methods: {
    onInput() {
      this.$emit("amount-input", this.localAmount);
    },
  },
};
</script>
<style scoped lang="scss">
.conversion__amount {
  padding: 10px;
  border: none;
  border-radius: 5px;
}
</style>
