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
      <tr v-for="print in prints">
        <td>
          <select v-model="print.size" v-bind:value="print.size">
            <option v-for="(value, key) in prices.print">{{ key }}</option>
          </select>
        </td>
        <td><input></input></td>
        <td>Druck</td>
        <td>Preis</td>
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
        <td>Summe:</td>
        <td>{{ priceSum }} Euro</td>
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
          A0: {
            A4a: 16,
            priceA4: 0.1,
            price: 1.6
          },
          A1: {
            A4a: 8,
            priceA4: 0.1,
            price: 0.8
          },
          A2: {
            A4a: 4,
            priceA4: 0.1,
            price: 0.4
          },
          A3: {
            A4a: 2,
            priceA4: 0.1,
            price: 0.2
          },
          A4: {
            A4a: 1,
            priceA4: 0.02,
            price: 0.02
          },
          A5: {
            A4a: 0.5,
            priceA4: 0.02,
            price: 0.01
          },
          A6: {
            A4a: 0.25,
            priceA4: 0.02,
            price: 0.01
          },
          A7: {
            A4a: 0.13,
            priceA4: 0.02,
            price: 0
          }
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
          double: false
        }
      }
    };
  },
  methods: {},
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
