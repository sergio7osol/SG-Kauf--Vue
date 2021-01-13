<template>
  <div class="buy-list pt-3">
    <button v-if="dateBuys.length" style="background-color: green; border-radius: 50%; color: white;">+</button>
    <ul class="list-group list-group-flush buy-list__items">
      <buy v-for="(buy, i) in dateBuys" :buy="buy" :key="i" />
      <!-- <li class="list-group-item buy-list__item" v-for="item in items" :key="item.date">
        <ul class="list-group buy-list__products">
          <li v-for="product in item.products" :key="product.name">
            <span class="mr-1">{{product.name}}</span> 
            <span class="mr-1">{{product.weightAmount}}</span> 
            <span class="mr-1">{{product.measure}}</span> 
          </li>
        </ul>
      </li> -->
    </ul>
  </div>
</template>

<script>
import Buy from './Buy.vue';

export default {
  name: 'buy-list',
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
    counter-reset: buy-counter;

    &__items {
      align-items: stretch;
      list-style-type: none;
    }
  } 
</style>
