<template>
  <nav id="sidebarMenu" class="d-md-block sidebar collapse vertical-menu">
      <div class="pt-3 pb-3">
        <ul class="nav flex-column vertical-menu__list">
          <li class="nav-item vertical-menu__item" v-for="item in datesSortedUp" :key="item.date">
            <span v-if="item.year" class="vertical-menu__item-year">{{item.year}}</span>
            <span v-if="item.month" class="vertical-menu__item-month">{{getMonthString(item.month)}}</span>
            <a v-if="item.date" 
              class="nav-link vertical-menu__item-link" 
              :class="{'vertical-menu__item-link--active': item.date === activeItem}" 
              @click.prevent="chooseDate(item.date)" 
              aria-current="page" 
              href="#"
            >
              <span class="vertical-menu__count-icon">{{item.count}}</span>
              <svg xmlns="http://www.w3.org/2000/svg" class="feather feather-shopping-cart vertical-menu__item-icon" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><circle cx="9" cy="21" r="1"></circle><circle cx="20" cy="21" r="1"></circle><path d="M1 1h4l2.68 13.39a2 2 0 0 0 2 1.61h9.72a2 2 0 0 0 2-1.61L23 6H6"></path></svg>
              <span class="vertical-menu__item-text">{{item.date}}</span>
            </a>
          </li>
        </ul>
      </div>
    </nav>
</template>

<script>
export default {
  name: 'left-menu',
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
    'date-selected' (payload) {
      if (typeof payload === 'string') return true; // TODO: to vadidate
    } 
    // 'end-sending': null
  },
  computed: {
    datesSortedUp() {
      const datesCopy = this.dates.slice();
      const sortedDates = Array.prototype.sort.call(datesCopy, (a, b) => {
        let aDate = null;
        let bDate = null;

        if (!a.date || !b.date) return 0;

        aDate = getDateInMillisec(a.date)
        bDate = getDateInMillisec(b.date);

        return aDate - bDate;
      });
      
      // go through and add year/month separators
      sortedDates.reduce((acc, v, i, src) => {
        let dateArr = null;
        let year = null;
        let month = null;

        if (v.date) {
          dateArr = v.date.split('.');
          year = dateArr[2];
          month = dateArr[1];

          if (acc.year !== year) {
            acc.year = year;
            src.splice(i, 0, { year });
          }

          if (acc.month !== month) {
            acc.month = month;
            src.splice(i, 0, { month });
          }
        }

        return acc;
      }, {year: null, month: null});

      return sortedDates;

      function getDateInMillisec(dateString) {
        const dateArr = dateString.split('.');
        const year = Number(dateArr[2]);
        const month = Number(dateArr[1]) - 1;
        const day = Number(dateArr[0]);
        const dateObj = new Date(year, month, day);
        const dateNumber = dateObj.getTime();

        return dateNumber;
      }
    }
  },
  methods: {
    chooseDate(date) {
      this.activeItem = date;
      this.$emit('date-selected', date);
    },
    getMonthString(monthNumber) {
      const monthNames = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
      monthNumber = Number(monthNumber) - 1;
      
      return monthNames[monthNumber];
    }
  },
  created: function () {
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
    background-color: #343a40;

    &__item {
      &--active {
        background-color: #dde;  
        border: 1px solid #bb88;
      }

      &-link {
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

      &-year, &-month {
        display: block;
        background-color: #dedede;
        color: #777;
        position: relative;
        z-index: 2;
        padding: .3rem 1rem;
        line-height: 1;
        border-bottom: 1px solid #fff;
        cursor: pointer;
      }

      &-year {
        font-size: .8rem;
      }

      &-month {
        font-size: 1rem;
      }
    }

    &__item-icon {
      position: absolute;
      left: 1rem;
    }

    &__count-icon {
      position: absolute;
      top: 0;
      bottom: 1.6rem;
      left: 1.8rem;
      margin-top: auto;
      margin-bottom: auto;
      height: 1.3rem;
      min-width: 1.3rem;
      border-radius: 1rem;
      background-color: #c92f2f;
      color: #fff;
      font-size: .7rem;
      line-height: 1.3rem;
      text-align: center;
      vertical-align: middle;
      z-index: 1;
    }
  }
</style>
