<template>
  <div class="home">
    <div v-for="(currency , index) in currencies" :key="index">
      <p class="currency" @click="viewCurrency(currency)">{{currency}}</p>
    </div>
     <modal name="currency-modal" @closed="modelCloseHandler">
        we are currently viewing {{currency}} modal
    </modal>
  </div>
</template>

<script>

export default {
  name: 'Home',
  data(){
    return {
      currencies: ['USD', 'AED' , 'GBP' , 'PKR'],
      currency: ''
    }
  },
  methods:{
    viewCurrency(currency){
      this.currency = currency
      this.$modal.show('currency-modal')
      window.history.pushState("", "", currency);
    },
    modelCloseHandler(){
      window.history.pushState("", "", '/');
      this.currency = {}
    }
  }
}
</script>

<style lang="css">
  .currency{
    color: green;
  }
  .currency:hover{
    cursor: pointer;
  }
</style>