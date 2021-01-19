<template>
    <div class="product product--default" v-if="isDefault">
        <product-info :info="infoFromProduct" :isDefault="isDefault" />
        <!-- <span class="badge bg-primary rounded-pill product__badge">{{item.products ? item.products.length: 0 }}</span> -->
    </div>
    <li class="product" v-else>
        <product-info :info="infoFromProduct" :isDefault="isDefault" />
    </li>
</template>

<script>
import ProductInfo from './ProductInfo.vue';

export default {
  name: 'product', 
  props: {
    product: Object,
    isDefault: Boolean
  },
  computed: {
      infoFromProduct() {
          const product = this.product;
          const info = {
            name: product.name, 
            weightAmount: product.weightAmount, 
            measure: product.measure,
            price: product.price, 
            description: product.description,
            discount: product.discount
        };

        return info;
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
