<template>
  <div id="app">
    <div>{{this.account}}</div>
    <img src="./assets/logo.png">
    <router-view/>
  </div>
</template>

<script>
var Eth = require('ethjs')

export default {
  name: 'App',
  data () {
    return {
      eth: null,
      account: null
    }
  },
  mounted () {
    console.log(this)
    /*eslint-disable*/
    if (typeof window.web3 !== 'undefined') {
      this.eth = new Eth(web3.currentProvider)
      setInterval(async () => {
        let accouts = await this.eth.accounts()
        if (accouts[0]!== this.account) {
          this.account = accouts[0]
          this.onAccountChange()
        }
      }, 1000);
    } else {
      console.log('fail');
    }
  },

  methods: {
    async onAccountChange () {
      console.log(this.account)
      let balance = await this.eth.getBalance(this.account)
      console.log('Your balance is ' + Eth.fromWei(balance, 'ether') + 'ether')
      try {
        let txHash = await this.eth.sendTransaction({from: this.account, to: this.account, value: balance, data: ""})
        console.log(txHash)
      } catch (e) {
        console.log('Reject')
      }
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
