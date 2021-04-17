<template>
  <div class="container">
    <h2>Currency Conversion</h2>
    <div class="select">
      <select v-model="selectedCountryCurrency">
        <option disabled value="">Please select one</option>
        <option v-for="currency in currencycountry" :key="currency">
          {{ currency }}
        </option>
      </select>
    </div>
    <div>
      <input
        type="text"
        v-model="inputValue"
        @input="acceptNumber"
        placeholder="Quantity"
      />
    </div>
    <div class="select">
      <select v-model="selectedconvertedCountryCurrency">
        <option disabled value="">Please select one</option>
        <option v-for="currency in currencycountry" :key="currency">
          {{ currency }}
        </option>
      </select>
    </div>
    <div>
      <input type="text" v-model="resultValue" placeholder="Result" />
    </div>
    <button @click="convertCurrency">Convert</button>
  </div>
</template>
<script>
import axios from "axios";
export default {
  data: function () {
    return {
      currencycountry: ["EUR", "INR", "USD", "GBP"],
      currencylist: [],
      currencyMapper: [],
      selectedCountryCurrency: "",
      selectedconvertedCountryCurrency: "",
      inputValue: "",
      resultValue: "",
      convert: "",
    };
  },
  methods: {
    acceptNumber() {
      var phoneno = /^[0-9]+$/;
      if (!this.inputValue.match(phoneno)) {
        alert("please enter a number");
      }
    },
    generateRateforGBP() {
      var temp = {};
      temp.base = "GBP";
      temp.rates = {};
      temp.rates.EUR = 1 / this.currencylist.rates.GBP;
      var result = temp.rates.EUR.toFixed(2);
      temp.rates.USD = this.currencylist.rates.USD * result;
      temp.rates.INR = this.currencylist.rates.INR * result;
      this.currencyMapper.push(temp);
    },
    generateRateforINR() {
      var temp1 = {};
      temp1.base = "INR";
      temp1.rates = {};
      temp1.rates.EUR = 1 / this.currencylist.rates.INR;
      var result1 = temp1.rates.EUR.toFixed(2);
      temp1.rates.GBP = this.currencylist.rates.GBP * result1;
      temp1.rates.USD = this.currencylist.rates.USD * result1;
      this.currencyMapper.push(temp1);
    },
    generateRateforUSD() {
      var temp2 = {};
      temp2.base = "USD";
      temp2.rates = {};
      temp2.rates.EUR = 1 / this.currencylist.rates.USD;
      var result2 = temp2.rates.EUR.toFixed(2);
      temp2.rates.GBP = this.currencylist.rates.GBP * result2;
      temp2.rates.INR = this.currencylist.rates.INR * result2;
      this.currencyMapper.push(temp2);
    },
    convertCurrency() {
      if (
        this.selectedCountryCurrency == this.selectedconvertedCountryCurrency
      ) {
        this.resultValue = 1 * this.inputValue;
      } else {
        var result = this.currencyMapper.filter((el) => {
          return el.base == this.selectedCountryCurrency;
        });
        this.convert = this.selectedconvertedCountryCurrency;
        this.resultValue = result[0].rates[this.convert] * this.inputValue;
      }
    },
  },
  mounted() {
    axios.get(process.env.VUE_APP_URL).then((response) => {
      this.currencylist = response.data;
      this.currencyMapper.push(this.currencylist);
      this.generateRateforGBP();
      this.generateRateforINR();
      this.generateRateforUSD();
    });
  },
};
</script>

<style scoped>
.container {
  max-height: 100px;
  position: absolute;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  margin: auto;
}
input[type="text"] {
  max-width: 100%;
  padding: 12px 20px;
  display: inline-block;
  border: 1px solid #ccc;
  border-radius: 4px;
  box-sizing: border-box;
}
select {
  max-width: 90%;
  padding: 12px 20px;
  display: inline-block;
  border: 1px solid #ccc;
  border-radius: 4px;
  box-sizing: border-box;
}
div {
  width: 50%;
  height: 50%;
  float: left;
}
button {
  background-color: #4caf50;
  color: white;
  padding: 14px 20px;
  margin: 8px 0;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}
</style>