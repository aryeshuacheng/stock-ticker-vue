<template>
  <center>
  <h1><b>Karma Stock Ticker!</b></h1>
  <input v-model="name" placeholder="Name" />
  <br>
  <input v-model="symbol" placeholder="Symbol" />
  <br>
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
      <td>{{stock.shares}}</td>
      <td> <button @click="buyStock">Buy</button></td>
      <td> <button @click="sellStock">Sell</button></td>
      <td> <button @click="removeFromPortfolio">Remove from Portfolio</button></td>
    </tr>
    </tbody>
  </table>
  </center>
</template>

<script>
export default {
  data() {
    return {
      name: '',
      stocks: [],
      symbol: 'GOOG'
    }
  },
  methods: {
    getQuotes() {
      fetch('http://localhost:3333/api/v1/get_quotes'+'?name=' + this.name)
        .then(response => response.json())
        .then(response => this.stocks = response.data)
    },
    addStockToPortfolio() {
      fetch('http://localhost:3333/api/v1/add_stock_to_portfolio?symbol=' + this.symbol + '&name=' + this.name)
    },
    removeFromPortfolio() {
      fetch('http://localhost:3333/api/v1/remove_stock_from_portfolio?symbol=' + this.symbol + '&name=' + this.name)
    },
    buyStock() {
      fetch('http://localhost:3333/api/v1/buy_stock?symbol=' + this.symbol + '&name=' + this.name)
    },
    sellStock() {
      fetch('http://localhost:3333/api/v1/sell_stock?symbol=' + this.symbol + '&name=' + this.name)
    },
  }
}
</script>
