<template>
    <div class="card border-primary mb-3 whole-sum" style="max-width: 18rem;">
        <!-- <div class="card-header">{{dateRange.start}} - {{dateRange.end}}</div> -->
        <div class="card-body text-info">
            <h5 class="card-title whole-sum__title">{{roundedAmount}}</h5> 
            <span class="whole-sum__currency">{{currency}}</span>
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
    amount: Number,
    currency: String
  }, 
  computed: {
      roundedAmount() {
          return this.amount.toFixed(2);
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
