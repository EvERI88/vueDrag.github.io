<template>
    <div class="addCards">
      <!-- <img alt="Vue logo" src="../assets/logo.png"> -->
      <!-- <HelloWorld msg="Welcome to Your Vue.js App"/> -->
      <AddCards />
    </div>
  </template>
  
  <script>
  // @ is an alias to /src
  import AddCards from '@/components/AddCards.vue'
  
  export default {
    name: 'AddCardsView',
    components: {
      AddCards,
    }
  }
  </script>
  