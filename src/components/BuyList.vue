<template>
  <div class="buy-list">
    <buy :buy="emptyBuy" isDefault />
    <ul class="list-group list-group-flush buy-list__items">
      <buy v-for="(buy, i) in dateBuys" :buy="buy" :key="buy.date + Date.now() + i" />
    </ul>
  </div>
</template>

<script>
import Buy from './Buy.vue';
 
export default {
  name: 'buy-list',
  data() {
    return {
      emptyBuy: {
        date: "",
        time: "00:00",
        currency: "EUR",
        country: "Germany",
        address: {
            city: "Hamburg",
            index: "",
            street: "",
            houseNumber: ""
        },
        payMethod: "EC card",
        shopName: "REWE",
        products: []
      }
    }
  },
  props: {
    dateBuys: Array
  },
  components: {
    Buy
  }
};
</script>

<style scoped lang="scss">
  .buy-list {
    height: 100vh;
    overflow-y: auto;
    overflow-x: hidden;
    padding-top: 3.5rem;

    &__items {
      counter-reset: buy-counter;
      align-items: stretch;
      list-style-type: none;

    }
  } 
</style>
