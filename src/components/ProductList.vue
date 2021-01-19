<template>
  <div class="col product-list pt-3 pl-3">
    <product :product="emptyProduct" :isDefault="true" />
    <ul class="list-group list-group-flush product-list__items">
      <product v-for="(product, i) in buyProducts" :isDefault="false" :product="product" :key="i" />
    </ul>
  </div>
</template>

<script>
import Product from './Product.vue';
 
export default {
  name: 'product-list',
  data() {
    return {
      emptyProduct: {
        name: "",
        description: "",
        weightAmount: "",
        measure: "piece",
        price: null,
        discount: null
      }
    }
  },
  props: {
    buyProducts: Array
  },
  components: {
    Product
  }
};
</script>

<style scoped lang="scss">
  .product-list {
    &__items {
      counter-reset: product-counter;
      align-items: stretch;
      list-style-type: none;
    }
  } 
</style>
