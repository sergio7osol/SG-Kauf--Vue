<template>
    <form class="buy-info">
        <div class="buy-info__date-and-time">
            <input class="form-control buy-info__date" min="2018-11-01" v-model="convertDate" :readonly="!edit" required type="date">
            <input class="form-control buy-info__time" v-model="time" minlength="5" maxlength="5" pattern="^[0-2][0-9]:[0-5][0-9]$" :readonly="!edit" type="text">
        </div>
        <div class="buy-info__address">
            <select class="form-control custom-select buy-info__country" v-model="country">
                <option v-for="land in countries" :key="land">{{land}}</option>
            </select>
            <select class="form-control custom-select buy-info__shop-name" v-model="shopName">
                <option v-for="name in shopNames" :key="name">{{name}}</option>
            </select>
            <select class="form-control custom-select buy-info__index" v-model="index">
                <option v-for="i in indexes" :key="i">{{i}}</option>
            </select>
            <select class="form-control custom-select buy-info__city" v-model="city">
                <option v-for="place in cities" :key="place">{{place}}</option>
            </select>
            <select class="form-control custom-select buy-info__street" v-model="street">
                <option v-for="str in streets" :key="str">{{str}}</option>
            </select>
            <select class="form-control custom-select buy-info__houseNumber" v-model="houseNumber">
                <option v-for="houseNr in houseNumbers" :key="houseNr">{{houseNr}}</option>
            </select>
        </div>
        <select class="form-control custom-select buy-info__currency" v-model="currency">
            <option v-for="currencyValue in currencies" :key="currencyValue">{{currencyValue}}</option>
        </select>
        <select class="form-control custom-select buy-info__pay-method" v-model="payMethod">
            <option v-for="method in payMethods" :key="method">{{method}}</option>
        </select>
        <button class="btn btn-primary btn-sm" @click="saveBuy">Submit</button>
    </form>
</template>

<script>
export default {
  name: 'buy-info',
  props: {
    info: Object 
  },
  data() {
      const inf = {
          ...this.info, 
          edit: true,
          countries: [
              'Germany',
              'Russia'
          ],
          cities: [
              'Hamburg',
              'Moscow',
              'Saransk'
          ],
          streets: [
              'Fuhlsbuettler Str.'
          ],
          shopNames: [
              'REWE'
          ],
          indexes: [
              '22307'
          ],
          houseNumbers: [
              '387'
          ],
          currencies: [
              'EUR',
              'RUB'
          ],
          payMethods: [
              'EC card',
              'Cash'
          ]
        };

      return inf;
  },
  computed: {
      convertDate: {
          get() {
              const currentDate = this.date;
              const normalizedDate = currentDate.split('.').reverse().join('-');

              return normalizedDate;
          },
          set(v) {
              const newDate = v;
              const normalizedDate = newDate.split('-').reverse().join('.');

              this.date = normalizedDate;
          }
      }
  },
  methods: {
      saveBuy() {
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
        let url = `http://localhost:3030/save-buy?date=${date}&time=${time}`;
        url += currency ? `&currency=${currency}` : '';
        url += country ? `&country=${country}` : '';
        url += city ? `&city=${city}` : '';
        url += index ? `&index=${index}` : '';
        url += street ? `&street=${street}` : '';
        url += houseNumber ? `&houseNumber=${houseNumber}` : '';
        url += payMethod ? `&payMethod=${payMethod}` : '';
        url += shopName ? `&shopName=${shopName}` : '';

        // console.log('RAW url >: ', url);
        // url = encodeURIComponent(url);

        console.log('url >: ', url);

        fetch(url)
            .then((response) => {
                if (response.status !== 200) {
                    console.log('Looks like there was a problem. Status Code: ' + response.status);
                    return;
                }
                
                response.json().then(function(data) {
                    console.log('RESP data: ', data);
                    thisApp.activeDateBuys = [...data];
                });
            })
            .catch(function(err) {
                console.log('Fetch Error :-S', err);
            });
        }
    },
    components: {
    }
} // Format: { date,  time, currency, address: { index, street, houseNumber }, payMethod, shopName, products: [] };
</script>

<style scoped lang="scss">
    .buy-info {
        display: flex;
        align-items: center;

        &::before {
            content: counter(buy-counter)".";
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
            width: 4.3rem;
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
                margin-right: .5rem;

                &:last-child {
                    margin-right: 0;
                }
            }
        }
    } 
</style>
