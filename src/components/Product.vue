<template>
    <div class="product product--default" v-if="isDefault">
        <product-info v-bind="product" isDefault @save-product="retriggerSendProductToSave" /> 
        <!-- <span class="badge bg-primary rounded-pill product__badge">{{item.products ? item.products.length: 0 }}</span> -->
    </div>
    <li class="product" v-else>
        <product-info v-bind="product" /> <!-- TODO: remove @save-product -->
    </li>
</template>

<script>
import ProductInfo from './ProductInfo.vue';

export default {
  name: 'product', 
  props: {
    product: {
        type: Object,
        required: true
    },
    isDefault: Boolean
  },
  emits: ['save-product'],
  methods: {
      retriggerSendProductToSave(product) {
          console.log('product: ', product);
          this.$emit('save-product', product);
      }
  },
  components: {
    ProductInfo
  }
} // Format: { date,  time, currency, address: { index, street, houseNumber }, payMethod, shopName, products: [] };
</script>

<style scoped lang="scss">
    .product {
        counter-increment: product-counter;
        padding-left: 2rem;

        &--default {
            margin-left: 2rem;
            
            .product-info::before {
                content: none;
            }
        }

        &__products {
            align-items: flex-start;
            margin-top: 1rem;
            font-size: .95rem;
            color: #565;
        }

        &__badge {
            color: #fff;
            position: absolute;
            top: 18px;
            right: 12px;
        }
    } 
</style>
