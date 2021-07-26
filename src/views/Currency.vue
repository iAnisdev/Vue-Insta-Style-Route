<template>
  <div class="currency">
    <h1>This is an {{currentRoute}} page</h1>
  </div>
</template>

<script>
export default {
  computed:{
    currentRoute(){
      return this.$route.params.currency
    }
  }
}
</script>
