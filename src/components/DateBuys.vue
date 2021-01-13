<template>
    <buy v-for="(buy, i) in buys" buy="buy" :key="i" />
</template>

<script>
import Buy from './Buy.vue';

export default {
  name: 'date-buys',
  props: {
    buys: Array 
  },
  components: {
      Buy
  }
}
</script>

<style scoped lang="scss">
</style>