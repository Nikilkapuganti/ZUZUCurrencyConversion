<template>
  <div id="app" >
    <currencyconverter />
  </div>
</template>

<script>
import currencyconverter from "../src/components/CurrencyConverter";

export default {
  name: "App",
  components: {
    currencyconverter,
  },
};
</script>

<style>
body {
  border-radius: 5px;
  background-color: #f2f2f2;
  text-align: center;
}
</style>
