<template>
  <tr class="col product-info">
      <th scope="col" v-if="isDefault">#</th>
      <th scope="row" class="product-list__number-col" v-else>{{index}}</th>

      <th scope="col" v-if="isDefault">
        <select class="form-control custom-select product-info__name" v-model="localName" :disabled="!isDefault">
          <option v-for="nameValue in ValueCollection.names" :key="nameValue + Date.now()">{{ nameValue }}</option>
        </select>
      </th>
      <td v-else>
        <select class="form-control custom-select product-info__name" v-model="localName" :disabled="!isDefault">
          <option v-for="nameValue in ValueCollection.names" :key="nameValue + Date.now()">{{ nameValue }}</option>
        </select>
      </td>

      <th scope="col" v-if="isDefault">
        <input class="form-control product-info__price" v-model="localPrice" placeholder="Price" step="0.01" :readonly="!isDefault" type="number" />
      </th>
      <td v-else>
        <input class="form-control product-info__price" v-model="localPrice" placeholder="Price" step="0.01" :readonly="!isDefault" type="number" />
      </td>

      <th scope="col" v-if="isDefault">
        <div class="product-info__weight-and-measure">
          <input class="form-control product-info__weight-amount" v-model="localWeightAmount" placeholder="Amount" step="0.001" :readonly="!isDefault" type="number" />
          <select class="form-control custom-select product-info__measure" v-model="localMeasure" :disabled="!isDefault">
              <option v-for="measureValue in ValueCollection.measures" :key="measureValue + Date.now()">{{ measureValue }}</option>
          </select>
        </div>
      </th>
      <td v-else>
        <div class="product-info__weight-and-measure">
          <input class="form-control product-info__weight-amount" v-model="localWeightAmount" placeholder="Amount" step="0.001" :readonly="!isDefault" type="number" />
          <select class="form-control custom-select product-info__measure" v-model="localMeasure" :disabled="!isDefault">
              <option v-for="measureValue in ValueCollection.measures" :key="measureValue + Date.now()">{{ measureValue }}</option>
          </select>
        </div>
      </td>
      
      <th scope="col" v-if="isDefault">
        <input class="form-control product-info__description" v-model="localDescription" placeholder="Description" :readonly="!isDefault" type="text" />
      </th>
      <td v-else>
        <input class="form-control product-info__description" v-model="localDescription" placeholder="Description" :readonly="!isDefault" type="text" />
      </td>

      <th scope="col" v-if="isDefault">
        <input class="form-control product-info__discount" v-model="localDiscount" placeholder="Discount" :readonly="!isDefault" type="text" /> <!-- pattern="d+\%?" -->
      </th>
      <td v-else>
        <input class="form-control product-info__discount" v-model="localDiscount" placeholder="Discount" :readonly="!isDefault" type="text" /> <!-- pattern="d+\%?" -->
      </td>

      <th scope="col" v-if="isDefault">
        <div class="product-info__buttons">
          <button class="btn btn--icon-remove" v-show="!isDefault" @click="sendProductToRemove" :readonly="!isDefault"></button>
          <button class="btn btn-success btn-sm product-info__btn-add text-nowrap" @click="sendProductToSave" :readonly="!isDefault">Save product</button>
        </div>
      </th>
      <td v-else>
        <div class="product-info__buttons">
          <button class="btn btn--icon-remove" v-show="!isDefault" @click="sendProductToRemove" :readonly="!isDefault"></button>
          <button class="d-none btn btn-success btn-sm product-info__btn-add text-nowrap" @click="sendProductToSave" :readonly="!isDefault">Save product</button>
        </div>
      </td>
  </tr>
</template>

<script>
export default {
  name: 'product-info',
  data() {
    const dataState = {
        localName: this.name,
        localDescription: this.description,
        localWeightAmount: this.weightAmount,
        localMeasure: this.measure,
        localPrice: this.price,
        localDiscount: this.discount,
        ValueCollection: {
          names: [],
          measures: ['piece', 'kg']      
        }
      };

      return dataState;
  },
  props: {
    isDefault: Boolean,
    index: Number,
    name: String,
    price: Number,
    weightAmount: Number,
    measure: {
      type: String,
      validator: function(value) {
          return ['piece', 'kg'].indexOf(value) !== -1;
      }
    },
    description: String,
    discount: [String, Number]
  },
  emits: ['save-product', 'remove-product'],
  methods: {
    sendProductToSave() {
      const product = {
        name: this.localName,
        price: this.localPrice,
        weightAmount: this.localWeightAmount,
        measure: this.localMeasure,
        description: this.localDescription,
        discount: this.localDiscount
      };

      console.log('sendProductToSave product: ', product);

      this.$emit('save-product', product);
    },
    sendProductToRemove() {
      const product = {
        name: this.localName,
        price: this.localPrice,
        weightAmount: this.localWeightAmount,
        measure: this.localMeasure,
        description: this.localDescription,
        discount: this.localDiscount
      };

      console.log('sendProductToRemove product: ', product);

      this.$emit('remove-product', product);
    },
    getProductNames() {
      const thisApp = this;

      fetch('http://localhost:3030/get-product-names')
        .then(
          function(response) {
            if (response.status !== 200) {
              console.log('Looks like there was a problem. Status Code: ' + response.status);
              return;
            }

            response.json().then(function(data) {
              if (!(data instanceof Array)) {
                console.log('Product names should be an Array of Strings. Got no products. Returns...');

                return false;
              }

              thisApp.ValueCollection.names = data;
            });
          }
        )
        .catch(function(err) {
          console.log('Fetch Error :-S', err);
        });
    }
  },
  created: function () {
    this.getProductNames();
  }
};
</script>

<style scoped lang="scss">
.product-info {
  // &:hover {
  //   background-color: #efefef;
  // }

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
    margin-right: 0.5rem;
  }

  &__discount {
    width: 6rem;
  }

  &__buttons {
    // width: 10rem;
    display: flex;
    justify-content: center;
    align-items: center;
  }
  &__btn-add {
    margin-left: .4rem;
  }

  th, td {
    vertical-align: middle;
  }
}

.product .form-control {
  height: 2rem;
  line-height: 1;
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
