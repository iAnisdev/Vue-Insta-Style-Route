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
      currency: '',
      questionIndex: 0
    }
  },
  methods:{
    viewCurrency(currency){
      this.currency = currency
      this.$modal.show('currency-modal')
      window.history.pushState({}, "", currency);
    },
    modelCloseHandler(){
      window.history.pushState({}, "", '/');
      this.currency = ''
    },
    PopStateEventHandler(event){
    let that = this
      if(event.currentTarget.location.pathname === '/'){
          that.currency = ''
          that.$modal.hide('currency-modal')
       }else{
        //  let targetCurrency = event.currentTarget.location.pathname.slice(1)
        //  console.log('targetCurrency' , targetCurrency ,event.currentTarget.location)
        //  if(that.currencies.indexOf(targetCurrency) !== -1){
        //    event.preventDefault()
        //     that.currency = that.currencies[that.currencies.indexOf(targetCurrency)]
        //     that.$modal.show('currency-modal')
        //     window.history.pushState({}, "", that.currency);
        //  }else{
        //    //let the route behave as normal
        //  }
       }
    }
  },
  mounted() {
     window.addEventListener('popstate', this.PopStateEventHandler);
 },
 destroyed(){
    window.removeEventListener('popstate' , this.PopStateEventHandler)
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