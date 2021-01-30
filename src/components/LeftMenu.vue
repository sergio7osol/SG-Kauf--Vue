<template>
  <nav id="sidebarMenu" class="d-md-block bg-dark sidebar collapse vertical-menu">
      <div class="position-sticky pt-3">
        <ul class="nav flex-column vertical-menu__list">
          <li class="nav-item vertical-menu__item" v-for="date in dates" :key="date">
            <a class="nav-link vertical-menu__item-link" :class="{'vertical-menu__item-link--active': date === activeItem}" @click="chooseDate(date)" aria-current="page" href="#">
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
    background-color: #2E2E2E;
    padding: 0;
    min-height: calc(100vh - 2.5rem);
    &__item {
      &--active {
        background-color: #dde;  
        border: 1px solid #bb88;
      }
    }
    &__item-link {
      display: flex;
      align-items: center;
      height: 61px;
      position: relative;
      z-index: 2;
      padding: 20px 30px 19px 70px;
      font-size: 18px;
      font-family: UniversLT-Condensed, "Helvetica Neue", Helvetica, Arial, "Lucida Grande", sans-serif;
      font-weight: normal;
      font-style: normal;
      color: #fff;
      text-decoration: none;
      text-transform: uppercase;
      border-bottom: 1px solid #777;
      transition: background-color 0.3s, color 0.3s;
      &::after {
        content: "";
        position: absolute;
        right: 0;
        border-color: transparent;
        border-width: 30px 0 30px 17px;
        border-style: solid;
        transition: right 0.3s 0.3s, border-color 0.3s;
      }

      &:hover {
        background: #232323;
        color: #0070BA;
      }

      &--active {
        background: #0070BA !important;
        color: #fff !important;

        &::after {
          border-color: #0000 #0000 #0000 #0070BA;
          right: -17px;
        }
      }
    }
    &__item-icon {
      position: absolute;
      left: 1rem;
    }
  }
</style>
