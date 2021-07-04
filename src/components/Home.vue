<template>
  <div class="wrapper">
      <div class="loader" v-if="loading">
        <Loader />
        Buying Otter..<a :href="`https://polygonscan.com/tx/${hash}`">TX</a>
      </div>  
      <div class="success" v-if="success">
        Purchase Was Successful!
      </div>   
              <div class="unlocked" v-if="!locked"> LIVE</div>
        <div class="locked" v-if="locked">NOT STARTED</div>
      <div class="portal">

      <div v-if="account" class="balance">Your Matic: {{balance}}</div>
      <div class="from"> 
        <span style="margin-left: 2.5em;">From</span> 
        <input type="text" inputmode="decimal" placeholder="0.0" pattern="^[0-9]*[.,]?[0-9]*$" v-model="input"   @keypress="isNumber($event)"  maxlength="5"> 
        Matic
      </div>
      <div class="to"> 
        To:
        {{output}}
        Otter
      </div>
      <div class="button">
        <div class="buy" v-if="account" @click="buy">Buy</div>
        <div class="buy" @click="$emit('connect')" v-if="!account">Connect</div>
      </div>
    </div>
    <div class="info"></div>
  </div>
</template>

<script>
import Loader from '../loaders/block.vue';
export default {
  name: 'Home',
  components:{
    Loader
  },
  data () {
    return {
      input: 0,
      hash: 0,
      loading: false,
      success: false,
    }
  },
  props:{
    account: String,
    balance: String,
    presale: Object,
    web3: Object,
    locked: Boolean
  },
  computed: {
    output: function() {
      let output = this.input / 0.027;
      return output.toFixed(2);
    }
  },
  methods: {
    isNumber: function(evt) {
      evt = (evt) ? evt : window.event;
      var charCode = (evt.which) ? evt.which : evt.keyCode;
      if ((charCode > 31 && (charCode < 48 || charCode > 57)) && charCode !== 46) {
        evt.preventDefault();
      } else {
        return true;
      }
    },
    buy: async function() {
      await this.presale.methods.Buy(this.account)
      .send(
        { from: this.account,
          value: this.web3.utils.toBN(this.input * 10 ** 18)
        }
      )
      .once("transactionHash", (hash) => {
        this.loading = true;
        this.hash = hash;
          console.log("Approval tx Hash:", hash);
        })
        .once("receipt", (receipt) => {
          this.loading = false;
          this.success = true;
          console.log("Approval Receipt:", receipt);
        });
    }
  },
  watch: {
    input: function () {
      console.log()
      if (parseFloat(this.input) > 500){
        this.input = 500;
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
.portal {
  margin: 2em auto;
  width: 20em;
  height: 20em;
  box-shadow: 0 0 1rem 0 rgba(0, 0, 0, 0.5); 
  border-radius: 20px;
  background-color: #30b2e6;
  backdrop-filter: blur(5px);
  position: relative;
  }
.button {
  position: absolute;
  bottom: 0;
  width: 100%;

}
.balance {
  position: absolute;
  top: 2em;
  left: 3.5em;
  color: white;
  font-size: 1.3em;
  font-weight: bold;
}
.from {
  top: 6em;
  width: 85%;
  color: #30b2e6;
  padding: .7em 0;
  background: white;
  border-radius: 2em;
  margin: 0 auto;
  font-weight: bold;
  border: 1px solid grey;
  position: relative;
  display: flex;
}
.to {
  top: 8em;
  width: 85%;
  color: #30b2e6;
  padding: .7em 0;
  background: white;
  border-radius: 2em;
  margin: 0 auto;
  font-weight: bold;
  border: 1px solid grey;
  position: relative;
}
input[type=text], select {
  width: 5em;
  border: none;
  font-size: 1.2em;
  border-radius: 4px;
  box-sizing: border-box;
  text-align: center;
}
.input {
    -webkit-writing-mode: horizontal-tb !important;
    text-rendering: auto;
    color: -internal-light-dark(black, white);
    letter-spacing: normal;
    word-spacing: normal;
    text-transform: none;
    text-indent: 0px;
    text-shadow: none;
    display: inline-block;
    text-align: start;
    appearance: auto;
    background-color: -internal-light-dark(rgb(255, 255, 255), rgb(59, 59, 59));
    -webkit-rtl-ordering: logical;
    cursor: text;
    margin: 0em;
    font: 400 13.3333px Arial;
    padding: 1px 2px;
    border-width: 2px;
    border-style: inset;
    border-color: -internal-light-dark(rgb(118, 118, 118), rgb(133, 133, 133));
    border-image: initial;
}
.buy {
  color: #30b2e6;
  padding: 1em 2em;
  width: 60%;
  border-radius: 2em;
  box-shadow: 0 0 1rem 0 rgba(0, 0, 0, 0.5); 
  background: white;
  margin: 1em auto;
  font-weight:bold;
  cursor: pointer;
}
.loader {
  background: rgba(0, 76, 148, 0.591);
  height: max-content;
  width: max-content;
  padding: 4em;
  border-radius: 2em;
   position: absolute;
   left: calc(50% - 9em);
   z-index: 99;
  top: 10em;
    margin: 0 auto;
      backdrop-filter: blur(5px);
    color: white;;
         box-shadow: 0 0 1rem 0 rgba(0, 0, 0, 0.5); 


}
.success {
  background: rgba(0, 148, 42, 0.53);
  height: max-content;
  width: max-content;
  padding: 2em;
  border-radius: 2em;
   position: absolute;
   left: calc(50% - 7em);
   z-index: 99;
   margin: 10em auto;
   color: white;
     box-shadow: 0 0 1rem 0 rgba(0, 0, 0, 0.5); 

  top: 2em;
}
.unlocked{
 margin:  auto;
 margin-top: 1em;;
  width: 15em;
background: rgb(26, 174, 26);
color: white;
border-radius: 2em;
}
.locked{
 margin:  auto;
 margin-top: 1em;;
  width: 15em;
background: rgb(116, 9, 9);
color: white;
border-radius: 2em;
}
</style>
