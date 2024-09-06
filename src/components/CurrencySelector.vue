<template>
  <div class="currency-selector">
    <select
      class="currency-selector__dropdown"
      :value="selectedCurrency"
      @change="onCurrencyChange"
    >
      <option
        class="currency-selector__option"
        v-for="currency in currencies"
        :key="currency"
        :value="currency"
      >
        {{ currency }}
      </option>
    </select>
  </div>
</template>

<script>
export default {
  props: {
    currencies: {
      type: Array,
      required: true,
    },
    selectedCurrency: {
      type: String,
      required: true,
    },
  },
  methods: {
    onCurrencyChange(event) {
      this.$emit("currency-changed", event.target.value);
    },
  },
};
</script>

<style scoped lang="scss">
.currency {
  &-selector {
    &__dropdown {
      width: 100%;
      padding: 5px;
      margin-bottom: 5px;
      border: none;
      border-radius: 5px;
    }
  }
  &__option {
  }
}
</style>
