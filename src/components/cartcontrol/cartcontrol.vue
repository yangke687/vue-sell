<template>
	<div class="cartcontrol">
		<div class="cart-decrease icon-remove_circle_outline" v-show="food.count>0"></div>
		<div class="cart-count" v-show="food.count>=0">{{ food.count }}</div>
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
				console.log('click', this.food.name);
				if (!this.food.count || typeof this.food.count === 'undefined') {
					Vue.set(this.food, 'cuont', 1);
				} else {
					this.food.count++;
				}
				console.log('count', this.food.count);
				this.$dispatch('cart.add', event.target);
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
		display:inline-block

</style>