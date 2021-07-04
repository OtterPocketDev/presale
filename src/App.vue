<template>
  <div id="app">
    <Header @connect="loadAccount" :account="account"/>
    <img width="400;" style="margin-top:2em;" alt="Vue logo" src="./assets/otter-logo.png">
    <Home @connect="loadAccount" :account="account" :balance="balance" :presale="presale" :web3="web3" :locked="locked"/>
  </div>
</template>

<script>
import Home from './components/Home.vue'
import Header from './components/header.vue';
import Web3 from "web3";
import Presale from "./abi/presale.json";

export default {
  name: 'App',
  components: {
    Home,
    Header
  },
  mounted() {
    this.loadWeb3();
  },
  data() {
    return {
      account: null,
      web3: null,
      connected: false,
      showPopup: false,
      balance: 0,
      network: 0,
      presale: null,
      locked: true
    }

  },
  methods: {
    loadWeb3: async function () {
      if (typeof window.ethereum !== 'undefined'){
        this.connected = true;
      } else {
        this.connected = false;
      }
    },
    loadAccount: async function () {
      let accounts = await window.ethereum.request({ method: 'eth_requestAccounts' });
      if(typeof window.ethereum !== 'undefined') {
        
      
        window.web3 = new Web3(window.ethereum);
        this.web3 = window.web3;
        this.network = await this.web3.eth.net.getId();
        this.presale = new this.web3.eth.Contract(
            Presale,
            "0x052b6f65673326A1b7dD8E339200Eec8887eeA8E"
          );
       this.locked = await this.presale.methods._locked().call();
       console.log('locked',await this.presale.methods._locked().call());
        if(this.network != 137){
          alert('Please Connect to the Polygon Network')
        } else {
          this.account = accounts[0];
          this.balance = await this.web3.eth.getBalance(
          this.account
        );
        this.balance = this.web3.utils.fromWei(
          this.balance,
          "ether"
        );
        this.balance = parseFloat(this.balance);
        this.balance = this.balance.toFixed(2);

        
        }

      } else {
        this.showPopup = true;
      }

    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 20px;
}
</style>
