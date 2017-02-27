<template>
	<div class="cartcontrol">
		<div class="cart-decrease icon-remove_circle_outline" v-show="food.count>0" @click="decreaseCart"></div>
		<div class="cart-count" v-show="food.count>0">{{ food.count }}</div>
		<div class="cart-increase icon-add_circle" @click="addCart"></div>
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
    .cart-decrease,.cart-increase
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