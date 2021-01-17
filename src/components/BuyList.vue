<template>
  <div class="buy-list pt-3">
    <buy :isDefault="true" :buy="emptyBuy" />
    <ul class="list-group list-group-flush buy-list__items">
      <buy v-for="(buy, i) in dateBuys" :buy="buy" :key="i" />
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
            index: "22307",
            street: "Fuhlsbuettler Str.",
            houseNumber: "387"
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
    &__items {
      counter-reset: buy-counter;
      align-items: stretch;
      list-style-type: none;
    }
  } 
</style>
