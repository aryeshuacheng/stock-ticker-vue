<template>
  <center>
  <h1><b>Karma Stock Ticker!</b></h1>
  <input v-model="name" placeholder="Player Name" />
  <br>

  <br>
  <br>
  <input v-model="symbol" placeholder="Symbol" />&nbsp&nbsp&nbsp<b-button variant="primary" @click="addStockToPortfolio">Add Stock</b-button>
  <br>
  <br>
  <b-button variant="primary" @click="getQuotes">Get Quotes</b-button>
  <br>
  <br>
  <b>Portfolio</b>
  <br>
  Cash Balance: ${{available_cash}}
  <br>
  <b-table-simple bordered small responsive>
    <b-thead>
    <b-tr variant="dark">
      <b-th scope="col">Symbol</b-th>
      <b-th scope="col">Price</b-th>
      <b-th scope="col">Change</b-th>
      <b-th scope="col">Shares</b-th>
      <b-th scope="col"></b-th>
      <b-th scope="col"></b-th>
      <b-th scope="col"></b-th>
    </b-tr>
    </b-thead>
    <b-tbody>
    <b-tr v-for="stock in stocks">
      <b-td>{{stock.symbol}}</b-td>
      <b-td>{{stock.latest_price}}</b-td>
      <b-td>{{stock.change}}</b-td>
      <b-td>{{stock.shares}}</b-td>
      <b-card>
      <b-td><b-button variant="success" @click="buyStock">Buy</b-button></b-td>
      <b-td><b-button variant="danger" @click="sellStock">Sell</b-button></b-td>
      <b-td><b-button @click="removeFromPortfolio">Remove from Portfolio</b-button></b-td>
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
      name: '',
      stocks: [],
      symbol: 'GOOG',
      available_cash: 0
    }
  },
  methods: {
    getQuotes() {
      fetch('http://localhost:3333/api/v1/get_quotes' + '?name=' + this.name)
          .then(response => response.json())
          .then(response => this.stocks = response.data)

      fetch('http://localhost:3333/api/v1/available_cash?name=' + this.name)
          .then(response => response.json())
          .then(response => this.available_cash = response)
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
    }
  }
}
</script>
