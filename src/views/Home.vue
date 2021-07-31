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
      currentId: 1,
      sequence: { prev: 0, curr: 0 }
    }
  },
  methods:{
    viewCurrency(currency){
      console.log('currency' , currency)
      this.currency = currency
      this.$modal.show('currency-modal')
      this.handlePageAdvancement(currency)
    },
    modelCloseHandler(){
      window.history.pushState({}, "", '/');
      this.currency = ''
    },
    PopStateEventHandler(event){
      var _to  = history.state ? history.state.sequence : false;
      var _from = this.sequence;
      console.log(this.sequence)
      if( !_to || _from.prev === _to.curr ){
        return  this.handleBackwards(event)
      }else {
        return this.handleForwards(event);
      }
    },
    handleBackwards(event){
      if(event.currentTarget.location.pathname === '/'){
          this.currency = ''
          this.$modal.hide('currency-modal')
       }
    },
    handleForwards(event){
        let targetCurrency = event.currentTarget.location.pathname.slice(1)
         if(this.currencies.indexOf(targetCurrency) !== -1){
            this.currency = this.currencies[this.currencies.indexOf(targetCurrency)]
            this.$modal.show('currency-modal');
            this.sequence = { prev: 0, curr: 0 }
            this.$router.push('/')
            this.handlePageAdvancement(targetCurrency)
            setTimeout(() => {
            this.viewCurrency(this.currencies[this.currencies.indexOf(targetCurrency)])
            }, 100);
            event.preventDefault();
         }else{
         }
    },
    handlePageAdvancement( newUrl ){
      this.sequence = { prev: this.sequence.curr, curr: this.sequence.curr+1 };
      history.pushState( { sequence: this.sequence }, null, newUrl );
    }
  },
  mounted() {
    window.addEventListener('popstate', this.PopStateEventHandler);
 },
 destroyed(){
   setTimeout(() => {
    window.removeEventListener('popstate' , this.PopStateEventHandler)
   }, 500);
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