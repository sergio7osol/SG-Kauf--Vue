<template>
  <div class="col product-list pt-3 pl-3">
    <product :product="emptyProduct" :isDefault="true" @save-product="retriggerSendProductToSave" :key="emptyProduct.measure + Date.now()" /> <!-- :key added, so that the product component always rerenders -->
    <ul class="list-group list-group-flush product-list__items">
      <product v-for="(product, i) in buyProducts" :isDefault="false" :product="product" :key="product.name + Date.now() + i" /> 
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
        name: null,
        description: "",
        weightAmount: null,
        measure: "piece",
        price: null,
        discount: null
      }
    }
  },
  props: {
    buyProducts: Array
  },
  emits: ['save-product'],
  methods: {
    retriggerSendProductToSave(product) {
          console.log('product-list: ', product);
          this.$emit('save-product', product);
      }
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
