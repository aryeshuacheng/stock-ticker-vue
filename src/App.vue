<template>
  <input v-model="message" placeholder="Symbol" />
  <br>
  <button @click="getQuote">Get Quote</button>
  <br>
  <h2 v-if="post">Symbol: {{ post.symbol }}</h2>
  <br>
  <h2 v-if="post">Price: {{ post.latest_price }}</h2>
  <br>
  <h2 v-if="post">Change: {{ post.change }}</h2>
</template>

<script>
export default {
  data() {
    return {
      post: [],
      message: 'GOOG'
    }
  },
  methods: {
    getQuote() {
      fetch('http://localhost:3333/api/v1/stocks?symbol=' + this.message)
        .then(response => response.json())
        .then(response => this.post = response.data)
    },
    postSymbol() {
      fetch('http://localhost:3333/api/v1/friends')
          .then(response => response.json())
          .then(response => this.post = response.data)
    }
  }
}
</script>
