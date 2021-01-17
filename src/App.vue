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
            <div class="col">
              <buy-list :dateBuys="activeDateBuys" />
            </div>
          </div>
        </div>
      </div>
      <div class="col-2 aside">
        <div class="weather-forecast">
          <h2>Aside column</h2>
        </div>
      </div>
    </div>
  </div> 
</template> 

<script>
import '../node_modules/normalize.css/normalize.css';
import '../node_modules/bootstrap/dist/css/bootstrap.min.css';
import LeftMenu from './components/LeftMenu.vue';
import BuyList from './components/BuyList.vue';

export default {
  name: 'App',
  data() {
    return {
      title: 'SG-Kauf--Vue',
      dates: [],
      activeDateBuys: [],
      start: null
    };
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
    }
  },
  mounted: function () {
    this.getAllDates();
  },
  components: {
    LeftMenu,
    // AddItem,
    BuyList,
    // DataSender
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
