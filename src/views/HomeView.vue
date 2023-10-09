<template>
  <div class="home">
    <Cards />
  </div>
</template>

<script>
// @ is an alias to /src
import Cards from '@/components/Cards.vue'
import AddCards from '@/components/AddCards.vue'

export default {
  name: 'HomeView',
  components: {
    Cards,
    AddCards,
  }
}
</script>
