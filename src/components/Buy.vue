<template>
    <div class="card buy-list__card--default" v-if="isDefault">
        <h5 class="card-header">New buy</h5>
        <div class="card-body">
            <div class="buy buy--default">
                <buy-info v-bind="buy" :isDefault="true" />
                <!-- <span class="badge bg-primary rounded-pill buy__badge">{{item.products ? item.products.length: 0 }}</span> -->
            </div>
        </div>
    </div>
    <li class="buy" v-else>
        <buy-info :info="infoFromBuy" />
    </li>
</template>

<script>
import BuyInfo from './BuyInfo.vue';

export default {
  name: 'buy', 
  props: {
    buy: {
        type: Object,
        required: true
    },
    isDefault: {
        type: Boolean,
        default: false
    }
  },
  computed: {
  }, 
  components: {
    BuyInfo
  }
} // Format: { date,  time, currency, address: { index, street, houseNumber }, payMethod, shopName, products: [] };
</script>

<style scoped lang="scss">
    .buy {
        counter-increment: buy-counter;

        &--default {
            .buy-info::before {
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
    .buy-list__card {
      &--default {
        background-color: #efefef;
        margin-bottom: 2rem;

        .card {
            &-header {
                text-align: left;
                font-weight: bold;
                color: #777;
            }
            &-body {
                padding: .8rem 0;
            }
        }
      }
    }
</style>
