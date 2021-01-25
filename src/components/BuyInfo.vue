<template>
  <form class="container-fluid">
    <div class="row">
        <div class="col buy-info">
            <div class="buy-info__date-and-time">
                <input class="form-control buy-info__date" min="2018-11-01" v-model="convertDate" required type="date" />
                <input class="form-control buy-info__time" v-model="localTime" type="time" />
            </div>
            <div class="buy-info__address">
                <select class="form-control custom-select buy-info__country" v-model="localCountry">
                    <option v-for="land in ValueCollection.countries" :key="land">{{ land }}</option>
                </select>
                <select class="form-control custom-select buy-info__shop-name" v-model="localShopName">
                    <option v-for="name in ValueCollection.shopNames" :key="name">{{ name }}</option>
                </select>
                <select class="form-control custom-select buy-info__index" v-model="localIndex">
                    <option v-for="i in ValueCollection.indexes" :key="i">{{ i }}</option>
                </select>
                <select class="form-control custom-select buy-info__city" v-model="localCity">
                    <option v-for="place in ValueCollection.cities" :key="place">{{ place }}</option>
                </select>
                <select class="form-control custom-select buy-info__street" v-model="localStreet">
                    <option v-for="str in ValueCollection.streets" :key="str">{{ str }}</option>
                </select>
                <select class="form-control custom-select buy-info__houseNumber" v-model="localHouseNumber">
                    <option v-for="houseNr in ValueCollection.houseNumbers" :key="houseNr">
                    {{ houseNr }}
                    </option>
                </select>
            </div>
            <select class="form-control custom-select buy-info__currency" v-model="localCurrency">
                <option v-for="currencyValue in ValueCollection.currencies" :key="currencyValue">
                    {{ currencyValue }}
                </option>
            </select>
            <select class="form-control custom-select buy-info__pay-method" v-model="localPayMethod">
                <option v-for="method in ValueCollection.payMethods" :key="method">{{ method }}</option>
            </select>
            <div class="buy-info__buttons"> 
                <button class="btn btn--icon-remove" v-show="!isDefault" @click="removeBuy"></button>
                <button class="btn btn-primary btn-sm buy-info__btn-add" @click="saveBuy">Add buy</button>
            </div>
        </div>
    </div>
    <div class="row" v-show="!isDefault">
        <product-list :buyProducts="localProducts" @save-product="saveProduct" @remove-product="removeProduct" />
    </div>
  </form>
</template>

<script>
import ProductList from './ProductList.vue';

export default {
  name: "buy-info",
  data() {
    const dataState = {
        localDate: this.date,
        localTime: this.time,
        localCurrency: this.currency,
        localCountry: this.country,
        localCity: this.city,
        localIndex: this.index,
        localStreet: this.street,
        localHouseNumber: this.houseNumber,
        localPayMethod: this.payMethod,
        localShopName: this.shopName,
        localProducts: this.products,
        ValueCollection: {
            countries: ["Germany", "Russia"],
            shopNames: ["REWE"], 
            indexes: ["22307"],
            cities: ["Hamburg", "Moscow", "Saransk"],
            streets: ["Fuhlsbuettler Str."],
            houseNumbers: ["387"],
            currencies: ["EUR", "RUB"],
            payMethods: ["EC card", "Cash"]
        }
    };

    console.log('dataState: ', dataState);

    return dataState;
  },
  props: {
    date: {
        type: String,
        // required: this.isDefault ? false : true // TODO
    },
    time: {
        type: String,
        required: true
    },
    currency: {
        type: String,
        validator: function(value) {
            return ["EUR", "RUB"].indexOf(value) !== -1;
        }
    },
    country: {
        type: String,
        validator: function(value) {
            return ["Germany", "Russia"].indexOf(value) !== -1;
        }
    },
    city: {
        type: String,
        validator: function(value) {
            return ["Hamburg", "Moscow", "Saransk"].indexOf(value) !== -1;
        }
    },
    index: {
        type: String,
        validator: function(value) {
            return ["22307"].indexOf(value) !== -1;
        }
    },
    street: {
        type: String,
        validator: function(value) {
            return ["Fuhlsbuettler Str."].indexOf(value) !== -1;
        }
    },
    houseNumber: {
        type: String,
        validator: function(value) {
            return ["387"].indexOf(value) !== -1;
        }
    },
    payMethod: {
        type: String,
        validator: function(value) {
            return ["EC card", "Cash"].indexOf(value) !== -1;
        }
    },
    shopName: {
        type: String,
        validator: function(value) {
            return ["REWE"].indexOf(value) !== -1;
        }
    },
    products: {
        type: Array,
        // validator: function(value) {
        //     return ["REWE"].indexOf(value) !== -1;
        // }
    },
    isDefault: Boolean
  },
  watch: {
  },
  computed: {
    convertDate: {
      get() {
        const currentDate = this.localDate;
        const normalizedDate = currentDate.split(".").reverse().join("-");

        return normalizedDate;
      },
      set(v) {
        const newDate = v;
        const normalizedDate = newDate.split("-").reverse().join(".");

        this.localDate = normalizedDate;
      }
    }
  },
  methods: {
    saveBuy() {
      let thisApp = this;
      let date = this.localDate;
      let time = this.localTime;
      let currency = this.localCurrency;
      let country = this.localCountry;
      let city = this.localCity;
      let index = this.localIndex;
      let street = this.localStreet;
      let houseNumber = this.localHouseNumber;
      let payMethod = this.localPayMethod;
      let shopName = this.localShopName;
    //   let products = this.localProducts;

      let url = `http://localhost:3030/save-buy?date=${date}&time=${time}`;
      url += currency ? `&currency=${currency}` : "";
      url += country ? `&country=${country}` : "";
      url += city ? `&city=${city}` : "";
      url += index ? `&index=${index}` : "";
      url += street ? `&street=${street}` : "";
      url += houseNumber ? `&houseNumber=${houseNumber}` : "";
      url += payMethod ? `&payMethod=${payMethod}` : "";
      url += shopName ? `&shopName=${shopName}` : "";

      console.log('RAW url >: ', url);
    //   url = encodeURIComponent(url);
    //   console.log('url >: ', url);

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
            thisApp.activeDateBuys = [...data];
          });
        })
        .catch(function (err) {
          console.log("Fetch Error :-S", err);
        });
    },
    removeBuy() {
    //   let thisApp = this;
      let date = this.localDate;
      let time = this.localTime;
      let url = `http://localhost:3030/remove-buy?date=${date}&time=${time}`;

      console.log("url >: ", url);

        if (confirm('You sure, you want to delete this buy?')) {

            console.log(`Prompted deleting of the buy. Confirmed. The buy on ${date} at ${time} is going to be deleted...`);
        } else {
            console.log(`Prompted deleting of the buy. Rejected. The buy on ${date} at ${time} is NOT going to be deleted.`);
            return false;
        }

      fetch(url)
        .then((response) => {
          if (response.status !== 200) {
            console.log("Looks like there was a problem. Status Code: " + response.status);
            return;
          }

          response.json().then(function (data) {
            // thisApp.activeDateBuys = [...data]; // TODO: emit
            console.log(`The buy on ${date} at ${time} was successfully removed.`, data);
          });
        })
        .catch(function (err) {
          console.log("Fetch Error :-S", err);
        });
    },
    saveProduct(product) {
        const thisApp = this;
        const date = this.localDate;
        const time = this.localTime;
        const { name, price, weightAmount, measure, description, discount } = product;
        let url = `http://localhost:3030/save-product?date=${date}&time=${time}`;
        url += name ? `&name=${name}` : '';
        url += price ? `&price=${price}` : '';
        url += weightAmount ? `&weightAmount=${weightAmount}` : '';
        url += measure ? `&measure=${measure}` : '';
        url += description ? `&description=${description}` : '';
        url += discount ? `&discount=${discount}` : '';

        console.log('RAW url >: ', url);
      // url = encodeURIComponent(url);

        fetch(url)
            .then((response) => {
                if (response.status !== 200) {
                    console.log("Looks like there was a problem. Status Code: " + response.status);
                    return;
                }

                response.json().then(function (data) {
                    if (response.status !== 200) {
                        console.log('Error. Program stops. ', data.error);
                        return false;
                    } else {
                        thisApp.localProducts = data;
                    }
                });
            })
            .catch(function (err) {
                console.log("Fetch Error :-S", err);
            });
    },
    removeProduct(product) {
        let thisApp = this;
        const date = this.localDate;
        const time = this.localTime;
        const { name, price, weightAmount, measure, description, discount } = product;
        let url = `http://localhost:3030/remove-product?date=${date}&time=${time}&name=${name}&price=${price}&weightAmount=${weightAmount}&measure=${measure}&discount=${discount}`;
        url += description ? `&description=${description}` : '';

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
                    
                    if (data.success === false) {
                        console.log('Error. Program stops. ', data.error);
                        return false;
                    } else {
                        console.log('thisApp.localProducts: ', JSON.stringify(thisApp.localProducts, null, 2));
                        thisApp.localProducts = data;
                        console.log('thisApp.localProducts 2: ', thisApp.localProducts);
                    }
                });
            })
            .catch(function (err) {
                console.log("Fetch Error :-S", err);
            });
    }
  },
  components: { 
      ProductList
  }
}; // Format: { date,  time, currency, address: { index, street, houseNumber }, payMethod, shopName, products: [] };
</script>

<style scoped lang="scss">
.buy-info {
  display: flex;
  align-items: center;
  justify-content: flex-end;

  &::before {
    content: counter(buy-counter) ".";
    margin-right: 1rem;
    font-size: 1.3rem;
    color: #ccc;
  }

  &__date-and-time {
    display: flex;
    margin-right: 1.5rem;
  }
  &__date {
    width: 10.5rem;
  }
  &__time {
    width: 8.3rem;
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

  &__buttons {
        width: 10rem;
        display: flex;
        justify-content: flex-end;
        align-items: center;
    }
    &__btn-add {
        width: 5.87rem;
        margin-left: .4rem;
    }
}

.buy--default .buy-info::before { content: none; }
.card {
    background-color: #ddd !important;
}
.btn {
  &--icon {
    &-remove {
      font-size: 2rem;
      color: #f00;
      padding-top: 0;
      padding-bottom: 0;
      line-height: 1.1;
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
