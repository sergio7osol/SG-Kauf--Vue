<template>
    <form class="col product-info">
        <select class="form-control custom-select product-info__name" v-model="localName">
          <option v-for="nameValue in ValueCollection.names" :key="nameValue + Date.now()">{{ nameValue }}</option>
        </select>
        <input class="form-control product-info__price" v-model="localPrice" placeholder="Price" step="0.01" type="number" />
        <div class="product-info__weight-and-measure">
          <input class="form-control product-info__weight-amount" v-model="localWeightAmount" placeholder="Amount" step="0.001" type="number" />
          <select class="form-control custom-select product-info__measure" v-model="localMeasure">
              <option v-for="measureValue in ValueCollection.measures" :key="measureValue + Date.now()">{{ measureValue }}</option>
          </select>
        </div>
        <input class="form-control product-info__description" v-model="localDescription" placeholder="Description" type="text" />
        <input class="form-control product-info__discount" v-model="localDiscount" placeholder="Discount" type="text" /> <!-- pattern="d+\%?" -->
        <div class="product-info__buttons">
          <button class="btn btn--icon-remove" v-show="!isDefault" @click="sendProductToRemove"></button>
          <button class="btn btn-success btn-sm product-info__btn-add" @click="sendProductToSave">Save product</button>
        </div>
    </form>
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
          names: ['Deli.Schw.Braten', 'Tilsiter 45% Sch', 'JA! Maasdamer', 'Steinofenbrot', 'Sonnenkern', 'Zitrone', 'Banane BIO', 'Heidelbeere', 'Obst/Gemuese', 'Salatgurke', 'Feine Butter', 'Wiedemilch 3.9%', 'Kefir Natur 1.5%', 'Speisequark Mag.', 'Ofenfr. BBQ-Pork', 'Schlaf+Nerventee', 'Miracel Whip', 'Gelee-Bananen', 'Interdentalb.Bl.', 'Pflaumenberliner', 'Berliner', 'Nußschnitte', 'Rohlfs Rusti', 'Haeh. Unterkeulen', 'Weihn. Chocolade', 'Santa Glamour', 'Gelee Baum', 'Pfeffersalami', 'Wacholderschinke', 'Muen. Weisswurst', 'JA! Emmentaler', 'JA! Mozzarella', 'Vital und fit', 'Muerbetorteletts', 'Rauecherlachs', 'Makrele geraeuch.', 'Ofenbaguette F.', 'Orange', 'Clementine', 'Mango', 'Kaki', 'Kartoffeln', 'Karotte', 'Rote Bete frisch', 'Eier Freil. Weiss', 'Kernige Flocken', 'Appel Heringsfil.', 'Hering Tomate', 'Hering Paprika', 'Ananasstücke', 'Classic Paprika', 'Sour Cream', 'Coca-Cola', 'Pfand', 'Rotk. Alkoholfrei', 'Lenor Weichspue', 'Somat Klarsp.', 'JA Spezialsalz', 'Leergut Einweg', 'Leerg. MW V. ST'],
          measures: ['piece', 'kg']      
        }
      };

      return dataState;
  },
  props: {
    isDefault: Boolean,
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
    }
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
