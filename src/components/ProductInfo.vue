<template>
    <form class="col product-info">
        <select class="form-control custom-select product-info__name" v-model="name">
          <option v-for="nameValue in names" :key="nameValue">{{ nameValue }}</option>
        </select>
        <div class="product-info__weight-and-measure">
          <input class="form-control product-info__weight-amount" v-model="weightAmount" :readonly="!edit" type="number" />
          <select class="form-control custom-select product-info__measure" v-model="measure">
              <option v-for="measureValue in measures" :key="measureValue">{{ measureValue }}</option>
          </select>
        </div>
        <!-- <div class="product-info__address">
          <select class="form-control custom-select product-info__shop-name" v-model="shopName">
              <option v-for="name in shopNames" :key="name">{{ name }}</option>
          </select>
          <select class="form-control custom-select product-info__index" v-model="index">
              <option v-for="i in indexes" :key="i">{{ i }}</option>
          </select>
          <select class="form-control custom-select product-info__city" v-model="city">
              <option v-for="place in cities" :key="place">{{ place }}</option>
          </select>
          <select class="form-control custom-select product-info__street" v-model="street">
              <option v-for="str in streets" :key="str">{{ str }}</option>
          </select>
          <select class="form-control custom-select product-info__houseNumber" v-model="houseNumber">
              <option v-for="houseNr in houseNumbers" :key="houseNr">
              {{ houseNr }}
              </option>
          </select>
        </div>
        <select class="form-control custom-select product-info__currency" v-model="currency">
        <option v-for="currencyValue in currencies" :key="currencyValue">
            {{ currencyValue }}
        </option>
        </select>
        <select class="form-control custom-select product-info__pay-method" v-model="payMethod">
        <option v-for="method in payMethods" :key="method">{{ method }}</option>
        </select> -->
        <button class="btn btn--icon-remove" v-show="!isDefault" @click="removeProduct"></button>
        <button class="btn btn-primary btn-sm" @click="saveProduct">Submit</button>
    </form>
</template>

<script>
export default {
  name: "product-info",
  data() {
    const infoProps = {...this.info};
    let data = {}; // new data object

    const defaultInfo = {
      edit: true,
      names: ['Pflaumenberliner', 'Berliner', 'Nußschnitte', 'Rohlfs Rusti'],
      measures: ["piece", "kg"]      
    };

    data = Object.assign({}, defaultInfo, infoProps);
    console.log('info data : ', data);

    return data;
  },
  props: {
    info: Object,
    isDefault: Boolean,
  },
  watch: {
      info(newInfo) {
          this.name = newInfo.name;
          this.weightAmount = newInfo.weightAmount;
          this.measure = newInfo.measure;
          this.price = newInfo.price;
          this.description = newInfo.description;
          this.discount = newInfo.discount;
      }
  },
  methods: {
    saveProduct() {
      let thisApp = this;
      let date = thisApp.date;
      let time = thisApp.time;
      let currency = thisApp.currency;
      let country = thisApp.country;
      let city = thisApp.city;
      let index = thisApp.index;
      let street = thisApp.street;
      let houseNumber = thisApp.houseNumber;
      let payMethod = thisApp.payMethod;
      let shopName = thisApp.shopName;
      let url = `http://localhost:3030/save-product?date=${date}&time=${time}`;
      url += currency ? `&currency=${currency}` : "";
      url += country ? `&country=${country}` : "";
      url += city ? `&city=${city}` : "";
      url += index ? `&index=${index}` : "";
      url += street ? `&street=${street}` : "";
      url += houseNumber ? `&houseNumber=${houseNumber}` : "";
      url += payMethod ? `&payMethod=${payMethod}` : "";
      url += shopName ? `&shopName=${shopName}` : "";

      // console.log('RAW url >: ', url);
      // url = encodeURIComponent(url);

      console.log("url >: ", url);

      fetch(url)
        .then((response) => {
          if (response.status !== 200) {
            console.log(
              "Looks like there was a problem. Status Code: " + response.status
            );
            return;
          }

          response.json().then(function (data) {
            console.log("RESP data: ", data);
            thisApp.activeDateProducts = [...data];
          });
        })
        .catch(function (err) {
          console.log("Fetch Error :-S", err);
        });
    },
    removeProduct() {
      let thisApp = this;
      let date = thisApp.date;
      let time = thisApp.time;
      let url = `http://localhost:3030/remove-product?date=${date}&time=${time}`;

      console.log("url >: ", url);

      fetch(url)
        .then((response) => {
          if (response.status !== 200) {
            console.log(
              "Looks like there was a problem. Status Code: " + response.status
            );
            return;
          }

          response.json().then(function (data) {
            console.log("REMOVE data: ", data);
            // console.log("REMOVE data: ", data);
          });
        })
        .catch(function (err) {
          console.log("Fetch Error :-S", err);
        });
    },
  }
}; // Format: { date,  time, currency, address: { index, street, houseNumber }, payMethod, shopName, products: [] };
</script>

<style scoped lang="scss">
.product-info {
  display: flex;
  align-items: center;

  &::before {
    content: counter(product-counter) ".";
    margin-right: 1rem;
    font-size: 1.3rem;
    color: #ccc;
  }

  &__name {
    width: 15rem;
    margin-right: 1.5rem;
  }
  &__weight-and-measure{
    display: flex;
    margin-right: 1.5rem;
  }

  &__address {
    display: flex;
    align-items: center;
  }
  &__country {
    width: 9rem;
  }
  &__shop-name {
    margin-right: 1.5rem;
    width: 6rem;
  }
  &__city {
    width: 8rem;
  }
  &__index {
    padding-left: 1rem;
    padding-right: 1rem;
    width: 6rem;
  }
  &__street {
    white-space: nowrap;
    padding-left: 1rem;
    padding-right: 1rem;
    width: 11rem;
  }
  &__houseNumber {
    padding-left: 1rem;
    padding-right: 1rem;
    width: 6rem;
    margin-right: 1.5rem;
  }
  &__currency {
    width: auto;
  }
  &__pay-method {
    padding-left: 1.5rem;
    padding-right: 1.5rem;
    width: 7.3rem;
    display: flex;
    margin-right: 1.5rem;

    &-check {
      padding-left: 0;
      margin-right: 0.5rem;

      &:last-child {
        margin-right: 0;
      }
    }
  }
}

.btn {
  &--icon {
    &-remove {
      font-size: 2rem;
      color: #f00;
      &:hover {
        color: lighten(#f00, 10%);
      }
      &:active {
        color: darken(#f00, 10%);
      }
      &::before {
        content: "\2718";
      }
    }
  }
}
</style>
