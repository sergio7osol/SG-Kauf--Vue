<template>
  <div class="buy-list">
    <h3>Buy list</h3>
    <ul class="list-group buy-list__items">
      <li class="list-group-item d-flex justify-content-between align-items-center buy-list__item" v-for="item in items" :key="item.date">
        <span class="badge bg-primary rounded-pill buy-list__badge">{{item.products ? item.products.length: 0 }}</span>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: 'buy-list',
  props: {
    items: Array
  },
};
</script>

<style scoped lang="scss">
  .buy-list {
    counter-reset: buy-counter;

    &__item {
      counter-increment: buy-counter;
      padding-left: 2rem;

      &:hover {
        background-color: #fefefe;
      }

      &::before {
        content: counter(buy-counter)".";
        margin-left: -1.5rem;
        font-size: 1.3rem;
        color: #ccc;
      }
    }

    &__badge {
      color: #fff;
    }
  } 
</style>
