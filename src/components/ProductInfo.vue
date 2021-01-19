<template>
    <form class="col product-info">
        <select class="form-control custom-select product-info__name" v-model="name">
          <option v-for="nameValue in names" :key="nameValue">{{ nameValue }}</option>
        </select>
        <input class="form-control product-info__price" v-model="price" placeholder="Price" :readonly="!edit" required type="number" />
        <div class="product-info__weight-and-measure">
          <input class="form-control product-info__weight-amount" v-model="weightAmount" :readonly="!edit" type="number" />
          <select class="form-control custom-select product-info__measure" v-model="measure">
              <option v-for="measureValue in measures" :key="measureValue">{{ measureValue }}</option>
          </select>
        </div>
        <input class="form-control product-info__description" v-model="description" placeholder="Description" :readonly="!edit" type="text" />
        <input class="form-control product-info__discount" v-model="discount" :readonly="!edit" type="number" />
        <div class="product-info__buttons">
          <button class="btn btn--icon-remove" v-show="!isDefault" @click="removeProduct"></button>
          <button class="btn btn-success btn-sm product-info__btn-add" @click="saveProduct">Add product</button>
        </div>
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
  justify-content: flex-end;
  padding: .5rem .7rem;

  &:hover {
    background-color: #efefef;
  }

  &::before {
    content: counter(product-counter) ".";
    margin-right: 1rem;
    font-size: 1.3rem;
    color: #ccc;
  }

  &__name {
    width: 25rem;
    margin-right: 1.5rem;
  }
  &__price {
    width: 6rem;
  }
  &__weight-and-measure{
    display: flex;
    margin-right: 1.5rem;
  }
  &__weight-amount {
    width: 6rem;
  }

  &__description {
    width: 15rem;
  }

  &__discount {
    width: 6rem;
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

.btn {
  &--icon {
    &-remove {
      font-size: 2rem;
      line-height: 1.1;
      color: #f00;
      padding-top: 0;
      padding-bottom: 0;
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
