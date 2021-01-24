<template>
    <div class="card border-primary mb-3 sum" style="max-width: 18rem;">
        <div class="card-header">{{date}}</div>
        <div class="card-body text-primary">
            <h5 class="card-title">{{amount}}</h5>
            <p class="card-text">Day</p>
        </div>
    </div>
</template>

<script>
export default {
  name: 'sum', 
  props: {
    amount: Number,
    date: String
  }
}
</script>

<style scoped lang="scss">
    .sum {
        &__title {
            font-size: 2rem;
        }
    } 
</style>
