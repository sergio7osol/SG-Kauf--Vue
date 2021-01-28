<template>
    <div class="card border-primary mb-3 whole-sum" style="max-width: 18rem;">
        <!-- <div class="card-header">{{dateRange.start}} - {{dateRange.end}}</div> -->
        <div class="card-body text-info">
            <h5 class="card-title whole-sum__title">{{roundedSum}}</h5> 
            <span class="whole-sum__currency">{{currency}}</span>
            <div class="whole-sum__payment mt-2">
                <div class="whole-sum__cost-col">
                    <span class="whole-sum__payment-name">Cost: </span>
                    <span class="whole-sum__payment-value">{{roundedCost}}</span>
                </div>
                <div class="whole-sum__cost-col">
                    <span class="whole-sum__payment-name">Discount: </span>
                    <span class="whole-sum__payment-value">{{roundedDiscount}}</span>
                </div>
            </div>
            <div class="card-body">
                <button class="btn btn-info whole-sum__submit" @click="sendGetWholeSum">Get whole Sum</button>
            </div>
        </div>
    </div>
</template>

<script>
export default {
  name: 'whole-sum', 
  props: {
    dateRange: Object, // {start: String, end: String}
    amount: Object,
    currency: String
  }, 
  computed: {
      roundedCost() {
          return this.amount.cost ? this.amount.cost.toFixed(2) : 0;
      }, 
      roundedDiscount() {
          return this.amount.discount ? this.amount.discount.toFixed(2) : 0;
      },
      roundedSum() {
          const cost = this.amount.cost || 0;
          const discount = this.amount.discount || 0;

          return (cost - discount).toFixed(2);
      }
  },
  emits: ['get-whole-sum'],
  methods: {
      sendGetWholeSum() {
          this.$emit('get-whole-sum');
      }
  }
}
</script>

<style scoped lang="scss">
    .whole-sum {
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
    } 
</style>
