<template>
  <center>
  <h1><b>Karma Stock Ticker!</b></h1>
  <input v-model="name" placeholder="Player Name"/>
  <br>
  <br>
  <br>
  <input v-model="symbol" placeholder="Symbol" />&nbsp&nbsp&nbsp<b-button variant="primary" @click="addStockToPortfolio">Add Stock</b-button>
  <br>
  <br>
  <b-button variant="primary" @click="getQuotes">Load Portfolio</b-button>
  <br>
  <br>
  <b>Portfolio</b>
  <br>
  Cash Balance: ${{available_cash}}
  <br>
  <b-table-simple bordered small responsive>
    <b-thead variant="primary">
    <b-tr >
      <b-th scope="col">Symbol</b-th>
      <b-th scope="col">Price</b-th>
      <b-th scope="col">Change</b-th>
      <b-th scope="col">Shares</b-th>
      <b-th scope="col">Actions</b-th>
      <b-th scope="col"></b-th>
      <b-th scope="col"></b-th>
      <b-th scope="col"></b-th>
    </b-tr>
    </b-thead>
    <b-tbody>
    <b-tr v-for="stock in stocks" :key="stock.api_data.symbol">
      <b-td>{{ stock.api_data.symbol }}</b-td>
      <b-td>{{ stock.api_data.latest_price}}</b-td>
      <b-td>{{ stock.api_data.change }}</b-td>
      <b-td>{{ stock.shares }}</b-td>
      <b-card><input placeholder="Quantity" v-model="shares_quantity[stock.api_data.symbol]" type="text" /><b-td><b-button variant="success" @click="buyStock(Object.keys(this.shares_quantity)[0], this.shares_quantity[Object.keys(this.shares_quantity)[0]])">Buy</b-button></b-td>
      <b-td><b-button variant="danger" @click="sellStock(Object.keys(this.shares_quantity)[0], this.shares_quantity[Object.keys(this.shares_quantity)[0]])">Sell</b-button></b-td>
      <b-td><b-button @click="removeFromPortfolio(stock.api_data.symbol)">Remove from Portfolio</b-button></b-td>
      </b-card>
    </b-tr>
    </b-tbody>
  </b-table-simple>
  </center>
</template>

<script>
export default {
  data() {
    return {
      name: 'Alan Cheng',
      shares_quantity: {},
      stocks: [],
      symbol: 'GOOG',
      available_cash: 0
    }
  },
  methods: {
    getQuotes() {
      fetch('http://localhost:3333/api/v1/get_quotes' + '?name=' + this.name)
          .then(response => response.json())
          .then(response => this.stocks = response)

      fetch('http://localhost:3333/api/v1/available_cash?name=' + this.name)
          .then(response => response.json())
          .then(response => this.available_cash = response)
    },
    addStockToPortfolio() {
      fetch('http://localhost:3333/api/v1/add_stock_to_portfolio?symbol=' + this.symbol + '&name=' + this.name)
    },
    removeFromPortfolio(symbol) {
      fetch('http://localhost:3333/api/v1/remove_stock_from_portfolio?symbol=' + symbol + '&name=' + this.name)
    },
    buyStock(symbol, quantity) {
      debugger
      fetch('http://localhost:3333/api/v1/buy_stock?symbol=' + symbol + '&name=' + this.name + '&shares_quantity=' + quantity)
    },
    sellStock(symbol, quantity) {
      fetch('http://localhost:3333/api/v1/sell_stock?symbol=' + symbol + '&name=' + this.name + '&shares_quantity=' + quantity)
    }
  }
}
</script>

