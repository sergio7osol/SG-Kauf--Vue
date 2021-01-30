<template>
    <product-info v-bind="product" isDefault @save-product="retriggerSendProductToSave" v-if="isDefault" /> 
    <product-info v-bind="product" :index="index + 1" @remove-product="retriggerSendProductToRemove" v-else /> <!-- TODO: remove @save-product -->
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
    index: Number,
    isDefault: Boolean
  },
  emits: ['save-product', 'remove-product'],
  methods: {
      retriggerSendProductToSave(product) {
          this.$emit('save-product', product);
      },
      retriggerSendProductToRemove(product) {
          console.log('product: ', product);
          this.$emit('remove-product', product);
      }
  },
  components: {
    ProductInfo
  }
}
</script>

<style scoped lang="scss">
    .product {
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
