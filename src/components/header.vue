<template>
  <div class="nav">
    <div class="connect" v-if="!account"  @click="$emit('connect')">Connect</div>
    
    <div class="account" v-if="account">
       <a
        :href="`https://polygonscan.com/address/${account}`"
        style="color: white; text-decoration:none;"
          >
        <div class="connect" >
                   
            {{ account.substring(0, 6) }}...{{
              account.substring(38, 42)
            }}
        </div>
                  </a>

    </div>

  </div>
</template>

<script>
export default {
  name: 'Home',
  props: {
    account: String,
    
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.nav {
    display:flex;
    justify-content: flex-end;
    padding: 0 2em;
    color: black;
}
.connect {
    font-weight: bold;
    color: white;
    letter-spacing: .3em;
    padding: .5em 1.5em;
    border-radius: 4em;
    background: #30B2E6;
    cursor: pointer;
}
.account {
    display: flex;
}
.balance {
    font-weight: bold;
    color: black;
    letter-spacing: .3em;
    padding: .5em 1.5em;
    border-radius: 4em;
    background: #daf5ff98;
    cursor: pointer;
    margin-right: 2em;
}
</style>
