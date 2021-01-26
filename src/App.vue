<template>
  <header class="navbar navbar-dark sticky-top bg-dark flex-md-nowrap p-0 shadow">
    <a class="navbar-brand col-md-3 col-lg-2 me-0 px-3" href="#">SG Kauf</a>
    <button class="navbar-toggler position-absolute d-md-none collapsed" type="button" data-bs-toggle="collapse" data-bs-target="#sidebarMenu" aria-controls="sidebarMenu" aria-expanded="false" aria-label="Toggle navigation">
      <span class="navbar-toggler-icon"></span>
    </button>
    <input class="form-control form-control-dark w-100" type="text" placeholder="Search" aria-label="Search">
    <ul class="navbar-nav px-3">
      <li class="nav-item text-nowrap">
        <a class="nav-link" href="#">Sign out</a>
      </li>
    </ul>
  </header>
  <div class="container-fluid main-wrapper">
    <div class="row">
      <div class="col main-content">
        <div id="spend-track">
          <div class="row">
            <div class="pl-0">
              <left-menu :dates='dates' @load-date='getDate' /> 
            </div>
            <div class="col mt-3">
              <buy-list :dateBuys="activeDateBuys" />
            </div>
          </div>
        </div>
      </div>
      <div class="col-2 mt-3 aside">
        <whole-sum :amount="wholeSum" :currency="activeCurrency" @get-whole-sum="getWholeSum" /> <!-- TODO: currency exchange      |:dateRange=""|         --> 
        <sum :date="activeDate" :amount="activeSum" :currency="activeCurrency" />
      </div>
    </div>
  </div> 
</template> 

<script>
import '../node_modules/normalize.css/normalize.css';
import '../node_modules/bootstrap/dist/css/bootstrap.min.css';
import LeftMenu from './components/LeftMenu.vue';
import BuyList from './components/BuyList.vue';
import WholeSum from './components/WholeSum';
import Sum from './components/Sum';

export default {
  name: 'App',
  data() {
    return {
      title: 'SG-Kauf--Vue',
      wholeSum: 0,
      dates: [],
      activeDateBuys: [],
      start: null
    };
  },
  computed: {
    activeDate() {
      try {
        return this.activeDateBuys[0].date;
      } catch (error) {
        return 'No date selected';
      }
    },
    activeSum() {
      const sum = this.activeDateBuys.reduce((buySum, buy) => {
        const products = buy.products;

        if (products) {
          buySum += products.reduce((productSum, product) => productSum += product.price * product.weightAmount, 0);
        }

        return buySum;
      }, 0);

      return sum;
    },
    activeCurrency() { // TODO: make converting to one currency for all buys (in case they are different)
      try {
        console.log('this.activeDateBuys[0]: ', this.activeDateBuys[0]);
        return this.activeDateBuys[0].currency;
      } catch (error) {
        return '';
      }
    }
  },
  methods: {
    getAllDates: function() {
      let dates = this.dates;

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
    },
    getDate: function(e) {
      let thisApp = this;
      let newDate = e;

      fetch(`http://localhost:3030/read-date?date=${newDate}`)
        .then(
          function(response) {
            if (response.status !== 200) {
              console.log('Looks like there was a problem. Status Code: ' + response.status);
              return;
            }

            response.json().then(function(data) {
              thisApp.activeDateBuys = data;
            });
          }
        )
        .catch(function(err) {
          console.log('Fetch Error :-S', err);
        });
    },
    getWholeSum() {
      const thisApp = this;

      fetch('http://localhost:3030/get-whole-sum')
        .then(
          function(response) {
            if (response.status !== 200) {
              console.log('Looks like there was a problem. Status Code: ' + response.status);
              return;
            }

            response.json().then(function(data) {
              const wholeSum = data.wholeSum;
              console.log('data wholE: ', wholeSum, typeof wholeSum);

              console.log('success!!!!!');

              if (wholeSum && typeof wholeSum === 'number') {
                thisApp.wholeSum = wholeSum;
              }
            });
          }
        )
        .catch(function(err) {
          console.log('Fetch Error :-S', err);
        });
    }
  },
  created: function () {
    this.getAllDates();
  },
  components: {
    LeftMenu,
    BuyList,
    Sum,
    WholeSum
  },
};
</script>

<style>
#spend-track {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
