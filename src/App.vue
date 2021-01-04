<template>
  <div class="container spend-track">
    <div class="row">
      <!-- <nav class="col-3" data-spend-track-wr="LeftMenu">
        <left-menu class="spend-track__left-menu" :items='buyItems' title="Vertical Menu" />
      </nav> -->
      <div class="col-9" data-spend-track-wr="Content">
          <div class="col-8">
            <!-- <data-sender btnColor="#c36f9e" @start-sending="started" @end-sending="ended" /> -->
            <h2>{{title}}</h2>
            <img alt="Vue logo" src="./assets/logo.png">
            <add-item />
            <buy-list :items="buyItems" />
          </div>
      </div>
    </div>
  </div>
</template>

<script>
import '../node_modules/bootstrap/dist/css/bootstrap.min.css';
// import LeftMenu from './components/LeftMenu.vue';
import BuyList from './components/BuyList.vue';
import AddItem from './components/AddItem.vue';
// import DataSender from './components/DataSender.vue';
// data:
import buyItems from './data/buy-list.json'; 

export default {
  name: 'App',
  data() {
    return {
      title: 'SG-Kauf--Vue',
      buyItems,
      start: null
    };
  },
  methods: {
    reverseMessage() {
      this.newText = this.newText
        .split('')
        .reverse()
        .join('');
    },
    started(e) {
      this.start = e;
      console.log('Start sending data: ', e);
    },
    ended() {
      const responseLasted = Date.now() - this.start;
      console.log('Finish sending data. Lasted: ', responseLasted);
    },
  },
  components: {
    // LeftMenu,
    AddItem,
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
  margin-top: 60px;
}
</style>
