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
          <button class="btn btn-success btn-sm product-info__btn-add text-nowrap" @click="sendProductToSave" :readonly="!isDefault">Save product</button>
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
          names: ['Deli.Schw.Braten', 'Tilsiter 45% Sch', 'JA! Maasdamer', 'Steinofenbrot', 'Sonnenkern', 'Zitrone', 'Banane BIO', 'Heidelbeere', 'Obst/Gemuese', 'Salatgurke', 'Feine Butter', 'Wiedemilch 3.9%', 'Kefir Natur 1.5%', 'Speisequark Mag.', 'Ofenfr. BBQ-Pork', 'Schlaf+Nerventee', 'Miracel Whip', 'Gelee-Bananen', 'Interdentalb.Bl.', 'Pflaumenberliner', 'Berliner', 'Nußschnitte', 'Rohlfs Rusti', 'Haeh. Unterkeulen', 'Weihn. Chocolade', 'Santa Glamour', 'Gelee Baum', 'Pfeffersalami', 'Wacholderschinke', 'Muen. Weisswurst', 'JA! Emmentaler', 'JA! Mozzarella', 'Vital und fit', 'Muerbetorteletts', 'Rauecherlachs', 'Makrele geraeuch.', 'Ofenbaguette F.', 'Orange', 'Clementine', 'Mango', 'Kaki', 'Kartoffeln', 'Karotte', 'Rote Bete frisch', 'Eier Freil. Weiss', 'Kernige Flocken', 'Appel Heringsfil.', 'Hering Tomate', 'Hering Paprika', 'Ananasstücke', 'Classic Paprika', 'Sour Cream', 'Coca-Cola', 'Pfand', 'Rotk. Alkoholfrei', 'Lenor Weichspue', 'Somat Klarsp.', 'JA Spezialsalz', 'Leergut Einweg', 'Leerg. MW V. ST', 'Farmerschinken', 'Cavi Art Classic', 'Fleisch Tortell.', 'Ananas /Costa Rica', 'Avocado Vorger.', 'Champignons', 'Elmex Gelee Gel', 'Glade Gel Lufterfrischer', 'Blätterteig Kirschkissen', 'Dragee Mandeln/Haselnüsse', 'Bledn-a-med Zahncreme', 'Makrelenf. geräuchert', 'Kandierter Ingwer, Sort.', 'Mousse au Chocolat', 'Königin Pasteten', 'Hackfleisch gemischt', 'Mumm Alkoholfrei', 'Rotk. Rose Alkoholfrei', 'Henkell Alkoholfrei', 'Sebamed Intim Waschgel', 'Durex Überrasch" Mich', 'Naturebox Spülung', 'Glade Autom. Spray', 'Domestos WC-Gel', 'Schmutzradierer', 'Frosch Oase Orange', 'Cillit Bang WC-St.', 'Hähnchen Unterschenkel', 'Räucherhaus Holz', 'Mütze Wmann', 'Schneewatte', 'White tree MIDI', 'Zierband WH', 'Weihnachtsdeko', 'Bilderrahmen 10x15', 'Höhrgerätbat813450', 'Weinkäse', 'Amarettino Dessert', 'Dilek Helva Pist.', 'Dilek Helva Kakao', 'KBio. Bandnudeln', 'Seeb. Walnusskerne', 'Ingwer', 'Lays Gesalzen', 'Pringles Original', 'Almondy Daim Torte', 'Mehrkornbrot', 'KLC Salami', 'Ungarische Salami'],
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
    }
  }
}; // Format: { date,  time, currency, address: { index, street, houseNumber }, payMethod, shopName, products: [] };
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

  &__description {
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
