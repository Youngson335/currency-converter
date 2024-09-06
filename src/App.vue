<template>
  <main class="content">
    <CurrencyConverter />
  </main>
</template>

<script>
import CurrencyConverter from "./components/CurrencyConverter.vue";

export default {
  components: {
    CurrencyConverter,
  },
};
</script>

<style>
* {
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}
select {
  font-family: "Wix Madefor Display", sans-serif;
  font-weight: 600;
}
#app {
  font-family: "Wix Madefor Display", sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: white;
  height: 100vh;
  background: linear-gradient(rgb(115, 182, 185), rgb(101, 136, 91));
}
.content {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 90%;
}
</style>
