<template>
  <v-row class="pt-8 pl-8" ols="12">
    <div cols="8">
      <Card></Card>

    </div>
    <div>
      <Ranking></Ranking>
    </div>

  </v-row>
  
</template>

<script>
// @ is an alias to /src
import Card from '@/components/Card.vue'
import Ranking from '../components/Ranking.vue'

export default {
  name: 'dashboard',
  components: {
    Card,
    Ranking
},

  data: () => ({
  }),
}
</script>
