<template>
	<div class="cartcontrol">
		<div class="cart-decrease" v-show="food.count>0" @click.stop.prevent="decreaseCart" transition="move">
      <span class="inner icon-remove_circle_outline"></span>
    </div>
		<div class="cart-count" v-show="food.count>0">{{ food.count }}</div>
		<div class="cart-increase icon-add_circle" @click.stop.prevent="addCart"></div>
	</div>
</template>

<script type="text/ecmascript-6">
	import Vue from 'vue';
	export default {
		props: {
			food: {
				type: Object
			}
		},
		methods: {
			addCart(event) {
				if (!event._constructed) {
					return;
				}
				if (!this.food.count) {
					Vue.set(this.food, 'count', 1);
				} else {
					this.food.count++;
				}
        this.$dispatch('cart.add', event.target);
			},
      decreaseCart(event) {
        if (!event._constructed) {
          return;
        }
        if (this.food.count) {
          this.food.count--;
        }
      }
		}
	};
</script>

<style lang="stylus" rel="stylesheet/stylus">
  .cartcontrol
    font-size: 0
    .cart-decrease
      display:inline-block
      padding: 6px
      transition: all .4s linear
      &.move-transition
        opacity: 1
        transform: translate3d(0,0,0)
        .inner
          display: inline-block
          line-height: 24px
          font-size: 24px
          color: rgb(0,160,220)
          transition: all .4s linear
          transform: rotate(0)
      &.move-enter
        opacity: 0
        transform: translate3d(24px,0,0)
        .inner
          transform: rotate(180deg)
      &.move-leave
        opacity: 0
        transform: translate3d(24px,0,0)
        .inner
          transform: rotate(180deg);
    .cart-increase
      display:inline-block
      padding: 6px
      line-height: 24px
      font-size: 24px
      color: rgb(0,160,220)
    .cart-count
      width: 12px
      display: inline-block
      font-size: 10px
      vertical-align: top
      padding-top: 6px
      line-height: 24px
      text-align: center
      color: rgb(147,153,159)
</style>