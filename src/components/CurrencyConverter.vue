<template>
  <div class="currency-converter">
    <h1 class="currency-converter__title">Конвертер валют</h1>

    <div class="currency-converter__controls">
      <AmountInput
        :amount="amount"
        @amount-input="updateAmount"
        class="currency-converter__amount-input"
      />

      <CurrencySelector
        :currencies="currencies"
        :selectedCurrency="baseCurrency"
        @currency-changed="updateBaseCurrency"
        class="currency-converter__currency-selector"
      />

      <CurrencySelector
        :currencies="currencies"
        :selectedCurrency="targetCurrency"
        @currency-changed="updateTargetCurrency"
        class="currency-converter__currency-selector"
      />
    </div>

    <ConversionResult
      :result="result"
      :targetCurrency="targetCurrency"
      class="currency-converter__result"
    />
  </div>
</template>

<script>
import axios from "axios";
import AmountInput from "./AmountInput.vue";
import CurrencySelector from "./CurrencySelector.vue";
import ConversionResult from "./ConversionResult.vue";

export default {
  components: {
    AmountInput,
    CurrencySelector,
    ConversionResult,
  },
  data() {
    return {
      amount: 0,
      baseCurrency: "",
      targetCurrency: "USD",
      currencies: [],
      rates: {},
      result: 0,
    };
  },
  methods: {
    async fetchCurrencies() {
      try {
        const response = await axios.get(
          "https://api.exchangerate-api.com/v4/latest/USD"
        );
        this.currencies = Object.keys(response.data.rates);
        this.rates = response.data.rates;
        this.baseCurrency = this.getDefaultBaseCurrency();
      } catch (error) {
        console.error("Ошибка при получении валют:", error);
      }
    },
    getDefaultBaseCurrency() {
      const locale = navigator.language || navigator.userLanguage;
      return locale.includes("ru") ? "RUB" : "USD";
    },
    updateAmount(newAmount) {
      this.amount = newAmount;
      this.convertCurrency();
    },
    updateBaseCurrency(newCurrency) {
      this.baseCurrency = newCurrency;
      this.convertCurrency();
    },
    updateTargetCurrency(newCurrency) {
      this.targetCurrency = newCurrency;
      this.convertCurrency();
    },
    convertCurrency() {
      if (!this.amount || !this.baseCurrency || !this.targetCurrency) {
        this.result = 0;
        return;
      }

      const baseRate =
        this.baseCurrency === "USD" ? 1 : this.rates[this.baseCurrency];
      const targetRate =
        this.targetCurrency === "USD" ? 1 : this.rates[this.targetCurrency];
      this.result = (this.amount / baseRate) * targetRate;
    },
  },
  mounted() {
    this.fetchCurrencies();
  },
};
</script>

<style scoped lang="scss">
.currency-converter {
  margin: auto;
  text-align: center;
  width: 100%;
  max-width: 500px;
  &__title {
    font-size: 40px;
    margin-bottom: 20px;
    font-weight: bold;
    text-align: start;
    font-weight: 900;
  }
  &__controls {
    display: flex;
    flex-direction: column;
    gap: 10px;
    margin-bottom: 20px;
  }
  &__result {
    font-size: 20px;
    font-weight: bold;
  }
}

.currency-converter__amount-input,
.currency-converter__currency-selector {
  width: 100%;
}
</style>
