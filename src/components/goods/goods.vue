<template>
  <div class="goods">
  <div class="menu-wrapper" v-el:menu-wrapper>
    <ul>
      <li v-for="item in goods" class="menu-item" :class="{'current': currentIndex===$index }" @click="selectMenu($index,$event)">
        <span class="text border-1px" >
        <span v-show="item.type>0" class="icon" :class="classMap[item.type]"></span>{{item.name}}
        </span>
      </li>
    </ul>
  </div>
  <div class="foods-wrapper" v-el:foods-wrapper>
    <ul>
      <li v-for="item in goods" class="food-list food-list-hook">
        <h1 class="title">{{item.name}}</h1>
        <ul>
          <li @click="selectFood(food,$event)" v-for="food in item.foods" class="food-item border-1px">
            <div class="icon">
              <img height="57" width="57" :src="food.icon" alt="">
            </div>
            <div class="content">
              <h2 class="name">{{ food.name }}</h2>
              <p class="desc">{{ food.description }}</p>
              <div class="extra">
                <span class="count">月售{{food.sellCount}}份</span><span>好评率{{food.rating}}%</span>
              </div>
              <div class="price">
                <span class="now">¥{{food.price}}</span><span class="old" v-show="food.oldPrice">¥{{food.oldPrice}}</span>
              </div>
              <div class="cartcontrol-wrapper">
                <cartcontrol :food="food"></cartcontrol>
              </div>
            </div>
          </li>
        </ul>
      </li>
    </ul>
  </div>
  <shopcart v-ref:shopcart :select-foods="selectFoods" :delivery-price="seller.deliveryPrice" :min-price="seller.minPrice"></shopcart>
  </div>
  <food :food="selectedFood" v-ref:food></food>
</template>
<script type="text/ecmascript-6">
  import BScroll from 'better-scroll';
  import shopcart from 'components/shopcart/shopcart';
  import cartcontrol from 'components/cartcontrol/cartcontrol';
  import food from 'components/food/food.vue';

  const ERR_OK = 0;

  export default {
    props: {
      seller: {
        type: Object
      }
    },
    created(){
      this.$http.get('/api/goods').then(function(res){
        res = res.body;
        if (res.errno === ERR_OK){
          this.goods = res.data;
          this.$nextTick(function(){
            this._initScroll();
            this._calculateHeight();
          });
        }
      });
      this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
    },
    data(){
      return {
        goods: [],
        heightList: [],
        scrollY: 0,
        selectedFood: {}
      };
    },
    computed: {
      // realtime functions
      currentIndex(){
        for (let i = 0; i < this.heightList.length; i++) {
          let h1 = this.heightList[i];
          let h2 = this.heightList[i + 1];
          if (!h2 || (this.scrollY >= h1 && this.scrollY < h2)) {
            return i;
          }
        }
        return 0;
      },
      selectFoods() {
        let foods = [];
        this.goods.forEach((good) => {
          good.foods.forEach((food) => {
            if (food.count) {
              foods.push(food);
            }
          });
        });
        return foods;
      }
    },
    methods: {
      selectMenu(idx, $event){
        if (!$event._constructed){
          // check if it is browser native event
          return;
        }
        let foodsList = this.$els.foodsWrapper.getElementsByClassName('food-list-hook');
        let el = foodsList[idx];
        this.foodsScroll.scrollToElement(el, 300);
      },
      selectFood(food, $event) {
        if (!$event._constructed) {
          return;
        }
        this.selectedFood = food;
        this.$refs.food.show();
      },
      _initScroll(){
        this.menuScroll = new BScroll(this.$els.menuWrapper, {
          click: true
        });
        this.foodsScroll = new BScroll(this.$els.foodsWrapper, {
          click: true,
          probeType: 3
        });
        this.foodsScroll.on('scroll', function(pos){
          this.scrollY = Math.abs(Math.round(pos.y));
        }.bind(this));
      },
      _calculateHeight(){
        let height = 0;
        this.heightList.push(height);
        let foodsList = this.$els.foodsWrapper.getElementsByClassName('food-list-hook');
        for (let i = 0; i < foodsList.length; i++) {
          height += foodsList[i].clientHeight;
          this.heightList.push(height);
        }
      },
      _drop(target) {
        this.$refs.shopcart.drop(target);
      }
    },
    components: {
      shopcart: shopcart,
      cartcontrol: cartcontrol,
      food: food
    },
    events: {
      'cart.add'(target){
        this._drop(target);
      }
    }
  };
</script>
<style lang="stylus" rel="stylesheet/stylus">

@import "../../common/stylus/mixin.styl"

.goods
  display: flex
  position: absolute
  top: 174px
  bottom: 46px
  overflow: hidden
  left: 0
  right: 0
  .menu-wrapper
    flex: 0 0 80px
    width: 80px
    background-color: #f3f5f7
    .menu-item
      display: table
      height: 54px
      width: 56px
      line-height: 14px
      padding 0 12px
      &.current
        position: relative
        margin-top: -1px
        background #FFF
        z-index: 10
        font-weight 700
        .text
          border-none()
      .icon
        display: inline-block
        width: 12px
        height: 12px
        margin-right: 2px
        vertical-align: middle
        background-size: 12px 12px
        background-repeat: no-repeat
        &.decrease
          bg-image('decrease_3')
        &.discount
          bg-image('discount_3')
        &.guarantee
          bg-image('guarantee_3')
        &.invoice
          bg-image('invoice_3')
        &.special
          bg-image('special_3')
      .text
        font-size: 12px
        display: table-cell
        vertical-align: middle
        border-1px(rgba(7,17,27,.1))
  .foods-wrapper
    flex: 1
    .title
      padding-left: 14px
      height: 26px
      line-height: 26px
      border-left: 1px solid #d9dde1
      font-size: 12px
      color: rgb(147,153,159)
      background: #f3f5f7
    .food-item
      display: flex
      margin: 18px
      border-1px(rgba(7,17,27,.1))
      padding-bottom: 18px
      &:last-child
        border-none()
        margin-bottom: 0;
      .icon
        flex: 0 0 57px
        margin-right: 10px
      .content
        flex: 1
        .name
          margin: 2px 0 8px 0;
          height: 14px
          line-height: 14px
          font-size: 14px
          color: rgb(7,17,27)
        .desc,.extra
          line-height: 10px
          font-size: 10px
          color: rgb(147,153,159)
        .desc
          line-height: 12px
          margin-bottom: 8px
        .extra
          .count
            margin-right: 12px
        .price
          font-weight: 700
          line-height: 24px
          .now
            margin-right: 8px
            font-size:14px
            color: rgb(240,20,20)
          .old
            text-decoration: line-throught
            font-size: 10px
            color: rgb(147,153,159)
        .cartcontrol-wrapper
          position: absolute
          right: 0
          bottom: 12px

</style>