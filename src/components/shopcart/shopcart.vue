<template>
	<div class="shopcart">
		<div class="content">
			<div class="content-left">
				<div class="logo-wrapper">
					<div class="logo" :class="{'highlight': totalCount>0}">
						<span class="icon-shopping_cart"></span>
					</div>
					<div v-show="totalCount>0" class="num" :class="{'highlight':totalCount}">{{totalCount}}</div>
				</div>
				<div class="price" :class="{'highlight':totalPrice>0}">¥ {{ totalPrice }}</div>
				<div class="desc">另需配送费 ¥ {{ deliveryPrice }}元</div>
			</div>
			<div class="content-right">
				<div class="pay" :class="{'enough': this.totalPrice>=this.minPrice}">{{payDesc}}</div>
			</div>
		</div>
	</div>
</template>

<script type="text/ecmascript-6">
export default {
	props: {
		deliveryPrice: {
			type: Number,
			default: 0
		},
		minPrice: {
			type: Number,
			default: 0
		},
		selectFoods: {
			type: Array,
			default() {
				return [{
					price: 10,
					count: 1
				}];
			}
		}
	},
	computed: {
		totalPrice() {
			let total = 0;
			this.selectFoods.forEach(function(food){
				total += food.price * food.count;
			});
			return total;
		},
		totalCount() {
			let count = 0;
			this.selectFoods.forEach((food) => {
				count += food.count;
			});
			return count;
		},
		payDesc() {
			if (this.totalPrice === 0) {
				return '¥' + this.minPrice + '元起配送';
			} else if (this.totalPrice < this.minPrice) {
				return '还差' + Math.abs(this.minPrice - this.totalPrice) + '元配送';
			} else {
				return '去结算';
			}
		}
	}
};
</script>

<style lang="stylus" rel="stylesheet/stylus">
	.shopcart
		position: fixed
		left: 0
		bottom: 0
		z-index: 50
		right: 0
		height: 48px
		.content
			display: flex
			background: #141d27
			.content-left
				flex: 1
				.logo-wrapper
					display: inline-block
					position: relative
					top: -10px
					margin: 0 12px
					padding: 6px
					width: 56px
					height: 56px
					box-sizing: border-box
					vertical-align: top
					border-radius: 50%
					background: #141d27
					.logo
						width: 100%
						height: 100%
						text-align: center
						background: #2b343c
						border-radius: 50%
						&.highlight
							background: rgb(0,160,220)
						.icon-shopping_cart
							line-height: 44px
						    font-size: 24px
						    color: #80858a
						    &.highlight
								color: #fff
					.num
						position: absolute;
						top: 0
						right: 0
						width: 24px
						height: 16px
						line-height: 16px
						text-align: center
						border-radius: 16px
						font-size: 9px
						font-weight: 700
						color: #FFF
						background: rgb(240,20,20)
						box-shadow: 0 4px 8px 0 rgba(0,0,0,.4)
				.price
					display: inline-block
					vertical-align: top
					margin-top: 12px
					line-height: 24px
					padding-right: 24px
					box-sizing: border-box
					border-right: 1px solid rgba(255,255,255,0.1)
					color: rgba(255,255,255,.4)
					font-size: 16px
					font-weight: 700
					&.highlight
						color: #FFF
				.desc
					display: inline-block
					vertical-align: top
					margin: 12px 0 0 12px
					line-height: 24px
					font-size: 10px
					color: rgba(255,255,255,.4)
			.content-right
				flex: 0 0 105px
				wdith: 105px
				.pay
					height: 48px
					line-height: 48px
					text-align: center
					font-size: 12px
					color: rgba(255,255,255,.4)
					font-weight: 700
					background: #2b333b
					&.enough
						background: #00b43c
						color: #FFF

</style>


