<template>
  <nav id="sidebarMenu" class="d-md-block bg-light sidebar collapse vertical-menu">
      <div class="position-sticky pt-3">
        <ul class="nav flex-column vertical-menu__list">
          <li class="nav-item vertical-menu__item" :class="{'vertical-menu__item--active': date === activeItem}" v-for="date in dates" :key="date">
            <a class="nav-link active vertical-menu__item-link" @click="chooseDate(date)" aria-current="page" href="#">
              <svg xmlns="http://www.w3.org/2000/svg" class="feather feather-shopping-cart vertical-menu__item-icon" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><circle cx="9" cy="21" r="1"></circle><circle cx="20" cy="21" r="1"></circle><path d="M1 1h4l2.68 13.39a2 2 0 0 0 2 1.61h9.72a2 2 0 0 0 2-1.61L23 6H6"></path></svg>
              <span class="vertical-menu__item-text">{{date}}</span>
            </a>
          </li>
        </ul>
      </div>
    </nav>
</template>

<script>
export default {
  name: 'Left Menu',
  data: function() { 
    return {
      activeItem: null
    }
  },
  props: {
    dates: {
      type: Array,
      required: true
    },
  },
  emits: {
      'load-date' (payload) {
        if (typeof payload === 'string') return true; // TODO: to vadidate
      } 
      // 'end-sending': null
    },
  methods: {
    chooseDate(date) {
      this.activeItem = date;
      this.$emit('load-date', date);
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
  h3 {
    margin: 40px 0 0;
  }
  .vertical-menu {
    padding: 0;
    &__item {
      border-bottom: 1px solid #aaa;
      &--active {
        background-color: #dde;  
        border: 1px solid #bb88;
      }
    }
    &__item-link {
      display: flex;
      padding-left: .5rem;
      justify-content: space-between;
    }
    &__item-icon {
     margin-right: .5rem;
    }
  }
</style>
