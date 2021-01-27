<template>
    <div class="card border-primary mb-3 sum" style="max-width: 18rem;">
        <div class="card-header">{{date}}</div>
        <div class="card-body text-primary">
            <h5 class="card-title sum__title">{{roundedSum}}</h5> 
            <span class="sum__currency">{{currency}}</span>
            <div class="sum__payment mt-2">
                <div class="sum__cost-col">
                    <span class="sum__payment-name">Cost: </span>
                    <span class="sum__payment-value">{{roundedCost}}</span>
                </div>
                <div class="sum__cost-col">
                    <span class="sum__payment-name">Discount: </span>
                    <span class="sum__payment-value">{{roundedDiscount}}</span>
                </div>
            </div>
            <p class="card-text sum__period">Day</p>
        </div>
    </div>
</template>

<script>
export default {
  name: 'sum', 
  props: {
    date: String,
    amount: Object, // TODO: validate
    currency: String
  }, 
  computed: {
      roundedCost() {
          return this.amount.cost.toFixed(2);
      }, 
      roundedDiscount() {
          return this.amount.discount.toFixed(2);
      },
      roundedSum() {
          return (this.amount.cost - this.amount.discount).toFixed(2);
      }
  }
}
</script>

<style scoped lang="scss">
    .sum {
        &__title {
            display: inline-block;
            font-size: 2rem;
        }
        &__currency {
            color: #ccc;
            font-size: 1.4rem;
            margin-left: .3rem;
            font-style: italic;
        }
        &__payment {
            display: flex;
            justify-content: space-between;
            border: 1px solid #ccc;
            padding: .1rem .4rem;
            margin-top: .7rem;

            &-name {
                font-size: .9rem;
                color: #bbb;
            }
        }
        &__period {
            margin-top: .8rem;
        }
    } 
</style>
