<template>
  <div class="currency-converter">
    <h1>Конвертер валют</h1>
    <div>
      <input
        type="number"
        v-model="amount"
        @input="convertCurrency"
        placeholder="Введите сумму"
      />
      <select v-model="baseCurrency" @change="convertCurrency">
        <option
          v-for="currency in currencies"
          :key="currency"
          :value="currency"
        >
          {{ currency }}
        </option>
      </select>
      <select v-model="targetCurrency" @change="convertCurrency">
        <option
          v-for="currency in currencies"
          :key="currency"
          :value="currency"
        >
          {{ currency }}
        </option>
      </select>
    </div>
    <div>
      <h2>Результат: {{ result }} {{ targetCurrency }}</h2>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      amount: 0,
      baseCurrency: this.getDefaultBaseCurrency(),
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
      } catch (error) {
        console.error("Ошибка при получении валют:", error);
      }
    },
    getDefaultBaseCurrency() {
      const locale = navigator.language || navigator.userLanguage;
      return locale.includes("ru") ? "RUB" : "USD"; // Пример для определения базовой валюты
    },
    convertCurrency() {
      if (this.amount && this.baseCurrency && this.targetCurrency) {
        const baseRate =
          this.baseCurrency === "USD" ? 1 : this.rates[this.baseCurrency];
        const targetRate =
          this.targetCurrency === "USD" ? 1 : this.rates[this.targetCurrency];
        this.result = (this.amount / baseRate) * targetRate;
      } else {
        this.result = 0;
      }
    },
  },
  mounted() {
    this.fetchCurrencies();
  },
};
</script>

<style scoped>
.currency-converter {
  max-width: 400px;
  margin: auto;
  text-align: center;
}
input,
select {
  margin: 10px 0;
  padding: 10px;
  width: 100%;
}
</style>
