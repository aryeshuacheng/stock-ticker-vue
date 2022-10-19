<template>
  <input v-model="symbol" placeholder="Symbol" />
  <br>
  <button @click="getQuote">Get Quote</button>
  <br>
  <h2 v-if="stock">Symbol: {{ stock.symbol }}</h2>
  <br>
  <h2 v-if="stock">Price: {{ stock.latest_price }}</h2>
  <br>
  <h2 v-if="stock">Change: {{ stock.change }}</h2>
</template>

<script>
export default {
  data() {
    return {
      stock: [],
      symbol: 'GOOG'
    }
  },
  methods: {
    getQuote() {
      fetch('http://localhost:3333/api/v1/stocks?symbol=' + this.symbol)
        .then(response => response.json())
        .then(response => this.stock = response.data)
    }
  }
}
</script>
