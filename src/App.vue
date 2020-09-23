<template>
  <div id="app">
    <div class="container">
      <div class="container__inner">
        <div class="sidebar">
          <div class="clear">
            <vs-button color="primary" type="filled" @click="clearFilters()"
              >Очистить фильтры</vs-button
            >
          </div>
          <div class="filters">
            <vs-collapse accordion>
              <vs-collapse-item class="filters__inner filters__inner--country">
                <div slot="header">
                  Страна
                </div>
                <vs-select autocomplete v-model="countryFilter" width="100%">
                  <vs-select-item
                    :key="index"
                    :value="item.name"
                    :text="item.name"
                    v-for="(item, index) in countries"
                  />
                </vs-select>
              </vs-collapse-item>

              <vs-collapse-item class="filters__inner filters__inner--type">
                <div slot="header">
                  Тип
                </div>
                <vs-select
                  multiple
                  autocomplete
                  v-model="typeFilter"
                  width="100%"
                >
                  <vs-select-item
                    :key="index"
                    :value="item"
                    :text="item"
                    v-for="(item, index) in types"
                  />
                </vs-select>
              </vs-collapse-item>
              <vs-collapse-item class="filters__inner filters__inner--rates">
                <div slot="header">
                  Звёзды
                </div>
                <vs-col v-for="(rate, index) in rates" :key="index">
                  <vs-checkbox v-model="ratesFilter" :vs-value="rate">
                    <vs-icon
                      icon="star"
                      v-for="star in rate"
                      :key="star"
                      color="primary"
                    ></vs-icon
                  ></vs-checkbox>
                </vs-col>
              </vs-collapse-item>
              <vs-collapse-item class="filters__inner filters__inner--reviews">
                <div slot="header">
                  Кол-во отзывов от
                </div>
                <vs-input-number v-model="reviewsFilter" />
              </vs-collapse-item>
              <vs-collapse-item class="filters__inner filters__inner--price">
                <div slot="header">
                  Цена до
                </div>
                <vs-slider :max="maxPrice" v-model="priceFilter" />
              </vs-collapse-item>
            </vs-collapse>
          </div>
          <div class="submit">
            <vs-button color="success" type="filled" @click="submitFilters()"
              >Применить фильтры</vs-button
            >
          </div>
        </div>
        <div class="main">
          <vs-table
            :data="hotels"
            max-items="3"
            pagination
            noDataText="Записей не найдено"
          >
            <template slot="thead">
              <vs-th>
                Название
              </vs-th>
              <vs-th>
                Описание
              </vs-th>
            </template>

            <template slot-scope="{ data }">
              <vs-tr :key="indextr" v-for="(tr, indextr) in data">
                <vs-td :data="data[indextr].name">
                  {{ data[indextr].name }}
                </vs-td>

                <vs-td :data="data[indextr].description">
                  {{ data[indextr].description }}
                </vs-td>

                <vs-td :data="data[indextr].id">
                  <vs-button color="primary" type="filled" size="small"
                    >Забронировать</vs-button
                  >
                </vs-td>
              </vs-tr>
            </template>
          </vs-table>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  data: () => ({
    countryFilter: "",
    typeFilter: [],
    ratesFilter: [],
    reviewsFilter: 0,
    priceFilter: null,
    types: [],
    rates: [5, 4, 3, 2, 1],
    hotels: [],
    countries: [],
    maxPrice: null
  }),
  methods: {
    fetchHotels() {
      // fetching hotels + making types array + getting max price for slider
      fetch(process.env.VUE_APP_HOTELS_API)
        .then(resp => resp.json())
        .then(data => {
          this.hotels = data.hotels;
          this.types = data.hotels
            .map(hotel => hotel.type)
            .filter((value, index, self) => self.indexOf(value) === index);
          this.maxPrice = Math.max.apply(
            Math,
            data.hotels.map(function(o) {
              return o.min_price;
            })
          );
        })
        .catch(error => console.error(error));
    },
    fetchCountries() {
      // fetching countries from exterenal json
      fetch(process.env.VUE_APP_COUNTRIES_API)
        .then(resp => resp.json())
        .then(data => (this.countries = data))
        .catch(error => console.error(error));
    },
    submitFilters() {
      let filtered = [];
      fetch(process.env.VUE_APP_HOTELS_API)
        .then(resp => resp.json())
        .then(data => {
          data.hotels.forEach(item => {
            let countryCond = this.countryFilter
              ? item.country == this.countryFilter
              : true;
            let typeCond = this.typeFilter.length
              ? this.typeFilter.includes(item.type)
              : true;
            let ratesCond = this.ratesFilter.length
              ? this.ratesFilter.includes(
                  Math.ceil(Number.parseFloat(item.rating))
                )
              : true;
            let reviewsCond = this.reviewsFilter
              ? item.reviews_amount >= this.reviewsFilter
              : true;
            let priceCond = this.priceFilter
              ? item.min_price <= this.priceFilter
              : true;
            if (
              countryCond &&
              ratesCond &&
              typeCond &&
              reviewsCond &&
              priceCond
            ) {
              filtered.push(item);
            }
          });
          this.hotels = filtered;
        })
        .catch(error => console.error(error));
    },
    clearFilters() {
      this.countryFilter = "";
      this.typeFilter = [];
      this.ratesFilter = [];
      this.reviewsFilter = 0;
      this.priceFilter = null;
      this.fetchHotels();
    }
  },
  mounted() {
    this.fetchHotels();
    this.fetchCountries();
  }
};
</script>

<style lang="scss">
@import "assets/scss/main";
</style>
