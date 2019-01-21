<template>
  <div class="hello">
    <h1>Preiskalkulator Druckaufträge</h1>
    Ein Designer wird benötigt:
    <input type="checkbox" v-model="settings.designer" /> <br />
    Versand über Büchertisch:
    <input type="checkbox" v-model="settings.shipping" />
    <h2>Drucksachen</h2>
    <table>
      <tr>
        <td>Größe</td>
        <td>Stück</td>
        <td>Druck</td>
        <td>Preis</td>
      </tr>
      <tr v-for="(print, key) in prints" :key="key">
        <td>
          <select
            v-model="print.size"
            v-bind:value="print.size"
            v-on:change="calcPrintPrice(print);"
          >
            <option v-for="(value, key) in prices.print" :key="key">{{
              key
            }}</option>
          </select>
        </td>
        <td>
          <input
            v-model="print.number"
            size="4"
            v-on:keyup="calcPrintPrice(print);"
          />
        </td>
        <td>
          <select
            v-model="print.double"
            v-bind:value="print.double"
            v-on:change="calcPrintPrice(print);"
          >
            <option value="false">Einseitig</option>
            <option value="true">Doppelseitig</option>
          </select>
        </td>
        <td>{{ print.price }} Euro</td>
      </tr>
    </table>

    <h2>Preiskalkulation</h2>
    <table>
      <tr>
        <td>Grundpauschale:</td>
        <td>{{ prices.base }} Euro</td>
      </tr>
      <tr>
        <td>Grafikerpauschale:</td>
        <td>{{ prices.designer * this.settings.designer }} Euro</td>
      </tr>
      <tr>
        <td>Versandkostenpauschale:</td>
        <td>{{ priceShipping }} Euro</td>
      </tr>
      <tr>
        <td>Druckkosten:</td>
        <td>Euro</td>
      </tr>
      <tr>
        <td><b>Summe:</b></td>
        <td>
          <b>{{ priceSum }} Euro</b>
        </td>
      </tr>
    </table>
  </div>
</template>

<script>
export default {
  name: "Calculator",
  data() {
    return {
      prices: {
        base: 15,
        designer: 50,
        shippingStandard: 10,
        shippingChurch: 5,
        print: {
          A0: 1.6,
          A1: 0.8,
          A2: 0.4,
          A3: 0.2,
          A4: 0.02,
          A5: 0.01,
          A6: 0.005,
          A7: 0.0025
        }
      },
      settings: {
        designer: true,
        shipping: true
      },
      prints: {
        0: {
          size: "A4",
          number: 100,
          double: false,
          basePrice: 0,
          price: 0
        }
      }
    };
  },
  methods: {
    calcPrintPrice: function(e) {
      if (e.size === "A0") e.basePrice = this.prices.print.A0;
      if (e.size === "A1") e.basePrice = this.prices.print.A1;
      if (e.size === "A2") e.basePrice = this.prices.print.A2;
      if (e.size === "A3") e.basePrice = this.prices.print.A3;
      if (e.size === "A4") e.basePrice = this.prices.print.A4;
      if (e.size === "A5") e.basePrice = this.prices.print.A5;
      if (e.size === "A6") e.basePrice = this.prices.print.A6;
      if (e.size === "A7") e.basePrice = this.prices.print.A7;
      if (e.double === true) e.price = e.basePrice * e.number * 2;
      else e.price = e.basePrice * e.number;
    }
  },
  computed: {
    priceSum: function() {
      return (
        this.prices.base +
        this.settings.designer * this.prices.designer +
        this.priceShipping
      );
    },
    priceShipping: function() {
      return (
        this.prices.shippingChurch * this.settings.shipping +
        this.prices.shippingStandard * !this.settings.shipping
      );
    }
  },
  created: function() {
    this.$http
      .get("https://jsonplaceholder.typicode.com/users")
      .then(function(response) {
        console.log(response.data);
      });
  }
};
</script>
