<template>
  <div id="app">
    <div class="container">
      <div class="container__inner">
        <div class="sidebar">
          <div class="clear">
            <vs-button color="primary" type="filled"
              >Очистить фильтры</vs-button
            >
          </div>
          <div class="filters">
            <vs-collapse accordion>
              <vs-collapse-item class="filters__inner filters__inner--country">
                <div slot="header">
                  Страна
                </div>
                <vs-select autocomplete v-model="select1">
                  <vs-select-item
                    :key="index"
                    :value="item.value"
                    :text="item.text"
                    v-for="(item, index) in options1"
                  />
                </vs-select>
              </vs-collapse-item>

              <vs-collapse-item class="filters__inner filters__inner--type">
                <div slot="header">
                  Тип
                </div>
                <vs-select multiple autocomplete v-model="select2">
                  <vs-select-item
                    :key="index"
                    :value="item.value"
                    :text="item.text"
                    v-for="(item, index) in options2"
                  />
                </vs-select>
              </vs-collapse-item>
              <vs-collapse-item class="filters__inner filters__inner--stars">
                <div slot="header">
                  Звёзды
                </div>
                <vs-col v-for="(region, index) in regions" :key="index">
                  <vs-checkbox v-model="form.regions" :vs-value="region">{{
                    region.name
                  }}</vs-checkbox>
                </vs-col>
              </vs-collapse-item>
              <vs-collapse-item class="filters__inner filters__inner--reviews">
                <div slot="header">
                  Кол-во отзывов от
                </div>
                <vs-input-number v-model="reviews" />
              </vs-collapse-item>
              <vs-collapse-item class="filters__inner filters__inner--price">
                <div slot="header">
                  Цена до
                </div>
                <vs-slider v-model="price" />
              </vs-collapse-item>
            </vs-collapse>
          </div>
          <div class="submit">
            <vs-button color="success" type="filled"
              >Применить фильтры</vs-button
            >
          </div>
        </div>
        <div class="main">
          <vs-table :data="users">
            <template slot="thead">
              <vs-th sort-key="email">
                Название
              </vs-th>
              <vs-th sort-key="username">
                Описание
              </vs-th>
              <vs-th sort-key="id">
                Nro
              </vs-th>
            </template>

            <template slot-scope="{ data }">
              <vs-tr :key="indextr" v-for="(tr, indextr) in data">
                <vs-td :data="data[indextr].email">
                  {{ data[indextr].email }}
                </vs-td>

                <vs-td :data="data[indextr].username">
                  {{ data[indextr].username }}
                </vs-td>

                <vs-td :data="data[indextr].id">
                  {{ data[indextr].id }}
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
    select1: "",
    select2: [],
    form: {
      regions: []
    },
    reviews: 333,
    price: 44,
    users: [
      {
        id: 1,
        name: "Leanne Graham",
        username: "Bret",
        email: "Sincere@april.biz",
        website: "hildegard.org"
      },
      {
        id: 2,
        name: "Ervin Howell",
        username: "Antonette",
        email: "Shanna@melissa.tv",
        website: "anastasia.net"
      },
      {
        id: 3,
        name: "Clementine Bauch",
        username: "Samantha",
        email: "Nathan@yesenia.net",
        website: "ramiro.info"
      },
      {
        id: 4,
        name: "Patricia Lebsack",
        username: "Karianne",
        email: "Julianne.OConner@kory.org",
        website: "kale.biz"
      },
      {
        id: 5,
        name: "Chelsey Dietrich",
        username: "Kamren",
        email: "Lucio_Hettinger@annie.ca",
        website: "demarco.info"
      },
      {
        id: 6,
        name: "Mrs. Dennis Schulist",
        username: "Leopoldo_Corkery",
        email: "Karley_Dach@jasper.info",
        website: "ola.org"
      },
      {
        id: 7,
        name: "Kurtis Weissnat",
        username: "Elwyn.Skiles",
        email: "Telly.Hoeger@billy.biz",
        website: "elvis.io"
      },
      {
        id: 8,
        name: "Nicholas Runolfsdottir V",
        username: "Maxime_Nienow",
        email: "Sherwood@rosamond.me",
        website: "jacynthe.com"
      },
      {
        id: 9,
        name: "Glenna Reichert",
        username: "Delphine",
        email: "Chaim_McDermott@dana.io",
        website: "conrad.com"
      },
      {
        id: 10,
        name: "Clementina DuBuque",
        username: "Moriah.Stanton",
        email: "Rey.Padberg@karina.biz",
        website: "ambrose.net"
      }
    ],
    options1: [
      { text: "Square", value: 1 },
      { text: "Rectangle", value: 2 },
      { text: "Rombo", value: 3 },
      { text: "Romboid", value: 4 },
      { text: "Trapeze", value: 5 },
      { text: "Triangle", value: 6 },
      { text: "Polygon", value: 7 },
      { text: "Regular polygon", value: 8 },
      { text: "Circumference", value: 9 },
      { text: "Circle", value: 10 },
      { text: "Circular sector", value: 11 },
      { text: "Circular trapeze", value: 12 }
    ],
    options2: [
      { text: "Square", value: 1 },
      { text: "Rectangle", value: 2 },
      { text: "Rombo", value: 3 },
      { text: "Romboid", value: 4 },
      { text: "Trapeze", value: 5 },
      { text: "Triangle", value: 6 },
      { text: "Polygon", value: 7 },
      { text: "Regular polygon", value: 8 },
      { text: "Circumference", value: 9 },
      { text: "Circle", value: 10 },
      { text: "Circular sector", value: 11 },
      { text: "Circular trapeze", value: 12 }
    ],
    regions: [
      { id: 1, name: "Queensland" },
      { id: 2, name: "South Australia" },
      { id: 3, name: "New South Wales" }
    ]
  })
};
</script>

<style lang="scss">
@import "assets/scss/main";
</style>
