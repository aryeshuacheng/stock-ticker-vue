<template>
  <center>
  <h1><b>Karma Stock Ticker!</b></h1>
  <input v-model="symbol" placeholder="Symbol" />
  <button @click="addStockToPortfolio">Add Stock</button>
  <br>
  <br>
  <button @click="getQuotes">Get Quotes</button>
  <br>
  <br>
  <b>Portfolio</b>
  <br>
  <br>
  <table class="table table-bordered table-striped">
    <thead>
    <tr>
      <th scope="col">Symbol</th>
      <th scope="col">Price</th>
      <th scope="col">Change</th>
    </tr>
    </thead>
    <tbody>
    <tr v-for="stock in stocks">
      <td>{{stock.symbol}}</td>
      <td>{{stock.latest_price}}</td>
      <td>{{stock.change}}</td>
    </tr>
    </tbody>
  </table>
  </center>
</template>

<script>
export default {
  data() {
    return {
      stocks: [],
      symbol: 'GOOG'
    }
  },
  methods: {
    getQuotes() {
      fetch('http://localhost:3333/api/v1/get_quotes')
        .then(response => response.json())
        .then(response => this.stocks = response.data)
        .then(response => console.log(response))
    },
    addStockToPortfolio() {
      fetch('http://localhost:3333/api/v1/add_stock_to_portfolio?symbol=' + this.symbol)
    }
  }
}
</script>
