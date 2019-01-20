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
        <td>{{ prices.print }} Euro</td>
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
        print: {}
      },
      settings: {
        designer: true,
        shipping: true
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
