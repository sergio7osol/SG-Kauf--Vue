<template>
  <nav id="sidebarMenu" class="col-lg-5 d-md-block bg-light sidebar collapse vertical-menu">
      <div class="position-sticky pt-3">
        <ul class="nav flex-column vertical-menu__list">
          <li class="nav-item vertical-menu__item" v-for="date in dates" :key="Date(date)">
            <a class="nav-link active vertical-menu__item-link" aria-current="page" href="#">
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
  data: function() { 
    return {
      title: 'Vertical Menu',
      dates: []
    }
  },
  props: {
    items: {
      type: Array,
      required: true
    },
  },
  methods: {
    getAllDates: function() {
      let dates = this.dates;
      console.log('dates 0: ', dates);

      fetch('http://localhost:3030/list-dates')
        .then(
          function(response) {
            if (response.status !== 200) {
              console.log('Looks like there was a problem. Status Code: ' + response.status);
              return;
            }

            response.json().then(function(data) {
              data.forEach(date => {
                dates.push(date);
              });
            });
          }
        )
        .catch(function(err) {
          console.log('Fetch Error :-S', err);
        });
    }
  },
  mounted: function () {
    this.getAllDates();
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
    }
    &__item-link {
     white-space: nowrap; 
    }
    &__item-icon {
     margin-right: .5rem;
    }
  }
</style>
