<template>
    <div class="buy buy--default" v-if="isDefault">
        <buy-info :info="infoFromBuy" />
        <!-- <span class="badge bg-primary rounded-pill buy__badge">{{item.products ? item.products.length: 0 }}</span> -->
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
    buy: Object,
    isDefault: Boolean
  },
  computed: {
      infoFromBuy() {
          const buy = this.buy;
          const info = {
            date: buy.date, 
            time: buy.time, 
            currency: buy.currency,
            country: buy.country, 
            city: buy.address.city,
            index: buy.address.index,
            street: buy.address.street,
            houseNumber: buy.address.houseNumber,
            payMethod: buy.payMethod,
            shopName: buy.shopName
        };

        return info;
      }
  }, 
  components: {
    BuyInfo,
    //   buyProducts
  }
} // Format: { date,  time, currency, address: { index, street, houseNumber }, payMethod, shopName, products: [] };
</script>

<style scoped lang="scss">
    .buy {
        counter-increment: buy-counter;
        padding-left: 2rem;
        margin-bottom: .5rem;

        &:hover {
            background-color: #fefefe;
        }

        &--default {
            margin-left: 2rem;
            margin-bottom: 1.5rem;
            
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
</style>
