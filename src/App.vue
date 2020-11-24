<template>
  <div id="app">
    <div class="lightbox" id="notice">
      <div class="content">
        <img :src="`${image}`" alt="" /><br /><br /><strong>{{ title }}</strong
        ><br /><br />{{ content }}<br />
      </div>
    </div>
    <div class="container">
      <div class="navbar">
        <form>
          <input
            class="searchInput"
            type="search"
            placeholder="請輸入內容..."
            aria-label="Search"
            v-model="search"
          />
          <button
            class="searchButtun"
            type="submit"
            @click="category = ''"
            @click.prevent="searchProduct()"
          >
            搜尋
          </button>
        </form>
      </div>

      <div class="slide">
        <div v-for="item in news">
          <div
            class="bgcovernews"
            :style="{ backgroundImage: `url(${item.img})` }"
          >
            <div class="preNext">
              <button class="button-prev" href="#" @click.prevent="prev()">
                <img
                  src="//akveo.github.io/eva-icons/outline/png/128/arrow-ios-back-outline.png"
                />
              </button>
              <button class="button-next" href="#" @click.prevent="next()">
                <img
                  src="//akveo.github.io/eva-icons/outline/png/128/arrow-ios-forward-outline.png"
                />
              </button>
              <div class="dot-group">
                <div @click="times = 0" @click.prevent="jump()"></div>
                <div @click="times = 1" @click.prevent="jump()"></div>
                <div @click="times = 2" @click.prevent="jump()"></div>
                <div @click="times = 3" @click.prevent="jump()"></div>
                <div @click="times = 4" @click.prevent="jump()"></div>
                <div @click="times = 5" @click.prevent="jump()"></div>
                <div @click="times = 6" @click.prevent="jump()"></div>
                <div @click="times = 7" @click.prevent="jump()"></div>
                <div @click="times = 8" @click.prevent="jump()"></div>
                <div @click="times = 9" @click.prevent="jump()"></div>
              </div>
            </div>
            <h2>
              <a href="#">{{ item.title }}</a>
            </h2>
          </div>
        </div>
      </div>

      <section class="clock">
        <p>倒數</p>
        <strong>
          <div class="colockbox" id="colockbox1">
            <span class="day">00</span>天 <span class="hour">00</span>小時
            <span class="minute">00</span>分 <span class="second">00</span>秒
          </div>
        </strong>
      </section>

      <div class="flex">
        <div class="list" style="width: 100px">
          <a
            class="listItem"
            data-toggle="list"
            href="#"
            @click="getProuducts()"
            @click.prevent="category = ''"
            >全部</a
          >
          <a
            class="listItem"
            data-toggle="list"
            href="#"
            @click="getProuducts()"
            @click.prevent="category = 'food'"
            >健康食品</a
          >
          <a
            class="listItem"
            data-toggle="list"
            href="#"
            @click="getProuducts()"
            @click.prevent="category = 'instrument'"
            >樂器</a
          >
          <a
            class="listItem"
            data-toggle="list"
            href="#"
            @click="getProuducts()"
            @click.prevent="category = 'clothes'"
            >衣服</a
          >
          <a
            class="listItem"
            data-toggle="list"
            href="#"
            @click="getProuducts()"
            @click.prevent="category = 'car'"
            >汽車</a
          >
        </div>
        <div class="grid">
          <a
            href="#"
            class="card button"
            style="padding: 0 3px"
            v-for="item in filterData"
            :key="item.id"
            @click="getProuducts()"
            @click.prevent="contentTamp(item)"
          >
            <div
              class="bg-cover"
              :id="`item${item.id}`"
              style="
                height: 150px;
                background-size: cover;
                background-position: center;
              "
              :style="{ backgroundImage: `url(${item.image})` }"
            ></div>
            <h5>{{ item.title }}</h5>
          </a>
        </div>
      </div>
    </div>
    <input
      type="button"
      class="backtotop"
      value="回頁首"
      @click.prevent="backtotop()"
    />
  </div>
</template>

<script>
import $ from "jquery";

export default {
  data() {
    return {
      search: "",
      category: "",
      product: [],
      content: "",
      title: "",
      image: "",
      news: "",
      times: 0,
    };
  },
  computed: {
    filterData() {
      const vm = this;

      const filterProducts = vm.product.filter(
        (word) => word.categoery === vm.category
      );

      if (vm.category) {
        return filterProducts;
      } else {
        return vm.product;
      }
    },
  },

  methods: {
    getProuducts() {
      const api = "https://next.json-generator.com/api/json/get/Nk-j31H5t";

      this.$http.get(api).then((response) => {
        this.product = response.data;
      });
    },

    getNews() {
      const api = "https://next.json-generator.com/api/json/get/NJL4scr5K";

      this.$http.get(api).then((response) => {
        this.news = response.data;
      });
    },

    contentTamp(itemObject) {
      this.content = itemObject.content;
      this.title = itemObject.title;
      this.image = itemObject.image;
      $(function () {
        $(".lightbox").css("display", "block");
      });
    },
    backtotop() {
      $("html,body").animate({ scrollTop: 0 }, 300);
    },
    searchProduct() {
      const tempProduct = [];
      const vm = this;

      this.product.forEach(function (item) {
        console.log(item);
        if (item.title.toLowerCase().includes(vm.search.toLowerCase())) {
          tempProduct.push(item);
          vm.product = tempProduct;
        }
      });
    },
    next() {
      const vm = this;

      if (vm.times > 8) {
        vm.time = 8;
      } else {
        vm.times = vm.times + 1;
      }

      $(function () {
        $(".bgcovernews").css("transform", `translateX(${-1140 * vm.times}px)`);
      });
    },
    prev() {
      const vm = this;
      if (vm.times < 1) {
        vm.time = 2;
      } else {
        vm.times = vm.times - 1;
      }
      $(function () {
        $(".bgcovernews").css("transform", `translateX(${-1140 * vm.times}px)`);
      });
    },
    jump() {
      const vm = this;

      $(function () {
        $(".bgcovernews").css("transform", `translateX(${-1140 * vm.times}px)`);
      });
    },
  },

  created() {
    this.getProuducts();
    this.getNews();
  },
};

document.onclick = function (e) {
  e.preventDefault;
  var id = e.srcElement.id;

  $(".lightbox").css("display", "none");
};

$(function () {
  countDown("2020/12/2 6:30:59", "#colockbox1");
});
function countDown(time, id) {
  var day_elem = $(id).find(".day");
  var hour_elem = $(id).find(".hour");
  var minute_elem = $(id).find(".minute");
  var second_elem = $(id).find(".second");
  var end_time = new Date(time).getTime();
  var sys_second = (end_time - new Date().getTime()) / 1000;
  var timer = setInterval(function () {
    if (sys_second > 1) {
      sys_second -= 1;
      var day = Math.floor(sys_second / 3600 / 24);
      var hour = Math.floor((sys_second / 3600) % 24);
      var minute = Math.floor((sys_second / 60) % 60);
      var second = Math.floor(sys_second % 60);
      $(day_elem).text(day);
      $(hour_elem).text(hour < 10 ? "0" + hour : hour);
      $(minute_elem).text(minute < 10 ? "0" + minute : minute);
      $(second_elem).text(second < 10 ? "0" + second : second);
    } else {
      clearInterval(timer);
    }
  }, 1000);
}
</script>

<style scoped lang="sass">
a
  text-decoration: none
  color: white
  background-color: rgba(0, 0, 50, 0.3)
  &:hover
    box-shadow: 2px 2px 2px 1px rgba(0, 0, 0, 0.2)
.lightbox
  position: fixed
  z-index: 30
  left: 0px
  top: 0px
  right: 0px
  bottom: 0px
  background: rgba(0, 0, 0, 0.4)
  display: none
  img
    z-index: 30
    margin-top: 30px
    width: auto
    height: auto
    overflow: scroll
  .content
    overflow: scroll
    width: 30%
    height: 100%
    background: #fff
    color: #333
    margin: auto
    position: absolute
    top: auto
    left: 0
    right: 0
    bottom: auto
    border: 3px solid #fff
    text-align: center
    strong
      font-size: 30px
.grid
  display: grid
  grid-template-columns: repeat(5, 27%)
  grid-column-gap: auto

.list
  margin-right: 30px

.flex
  display: flex

.listItem
  display: block
  margin-bottom: 10px
  padding: 5px
  text-decoration: none
  background: rgb(251, 53, 53)
  text-align: center
  border-radius: 5px
  color: antiquewhite

.card
  height: 200px
  width: 150px
  border: 1px solid gray
  margin-bottom: 20px
  text-align: center

.listItem:hover
  text-shadow: 1px 1px 2px rgba(139, 139, 139, 0.479)
  color: rgb(255, 255, 255)
  background: rgb(216, 33, 33)

.navbar
  background-color: #2c3e50
  height: 70px
  display: flex
  align-items: center
  justify-content: center
  position: sticky
  top: 0
  z-index: 29

.clock
  background-color: #dd7777
  height: 50px
  display: flex
  align-items: center
  justify-content: center
  margin-bottom: 10px

.container
  margin-right: auto
  margin-left: auto
  padding-right: 15px
  padding-left: 15px
  width: 100%
  max-width: 1140px

.searchInput
  height: 50px
  width: 200px
  font-size: 22px
  border-radius: 0.5rem

.searchButtun
  height: 50px
  width: 200px
  font-size: 22px
  margin-left: 10px
  border-radius: 0.5rem
  background-color: rgb(255, 72, 72)

  &:hover
    background-color: rgb(133, 30, 30)
    color: beige

.slide

  text-align: center
  color: white

  display: flex
  overflow: hidden

.bg-cover
  background-size: cover
  background-position: center center
  height: 300px
  margin-top: 5px
  margin-bottom: 10px
  width: 100%

  &-card
    background-size: cover

    background-position: center center
    height: 100px
    margin-top: 5px
    margin-bottom: 10px

.bgcovernews
  background-size: cover
  background-position: center center
  transform: translateX(0)
  margin-top: 5px
  margin-bottom: 10px

  h2
    width: 1140px
.backtotop
  background-color: rgba(82, 173, 115,0.7)
  width: 50px
  height: 50px
  position: fixed
  bottom: 60px
  right: 60px

.button-prev, .button-next

  z-index: 1
.button-prev, .button-next

  top: 20%

  transition: opacity .3s
  &:hover
    opacity: .7
  img
    display: block
    max-width: 50px

.preNext
  display: flex

.dot-group
  margin-left: 50px
  right: 0
  bottom: 8px
  left: 0
  display: flex
  justify-content: center
  div
    margin-right: 10px
    height: 30px
    width: 50px
    background-color: white
    opacity: .7
    &:hover
      opacity: 1
</style>


