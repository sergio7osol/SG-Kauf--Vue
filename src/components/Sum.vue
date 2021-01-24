<template>
    <div class="card border-primary mb-3 sum" style="max-width: 18rem;">
        <div class="card-header">{{date}}</div>
        <div class="card-body text-primary">
            <h5 class="card-title sum__title">{{roundedAmount}}</h5> 
            <span class="sum__currency">{{currency}}</span>
            <p class="card-text">Day</p>
        </div>
    </div>
</template>

<script>
export default {
  name: 'sum', 
  props: {
    date: String,
    amount: Number,
    currency: String
  }, 
  computed: {
      roundedAmount() {
          return this.amount.toFixed(2);
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
    } 
</style>
