<template>
	<div class="shopcart">
		<div class="content" @click="toggle">
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
				<button class="pay" :class="{'enough': this.totalPrice>=this.minPrice}" :disabled="this.totalPrice<this.minPrice">{{payDesc}}</button>
			</div>
		</div>
		<div class="ball-container">
			<div v-for="ball in balls" v-show="ball.show" class="ball" transition="drop">
				<div class="inner inner-hook"></div>
			</div>
		</div>
		<div class="shopcart-list" v-show="listShow">
			<div class="list-header">
				<h1 class="title">购物车</h1>
				<span class="empty">清空</span>
			</div>
			<div class="list-content">
				<ul>
					<li class="food" v-for="food in selectFoods">
						<span class="name">{{food.name}}</span>
						<div class="price">
							<span>¥{{food.price*food.count}}</span>
						</div>
						<div class="cartcontrol-wrapper">
							<cartcontrol :food="food"></cartcontrol>
						</div>
					</li>
				</ul>
			</div>
		</div>
	</div>
</template>

<script type="text/ecmascript-6">
import cartcontrol from 'components/cartcontrol/cartcontrol.vue';
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
	data() {
		return {
			balls: [
				{
					id: 0,
					show: false
				},
				{
					id: 1,
					show: false
				},
				{
					id: 2,
					show: false
				},
				{
					id: 3,
					show: false
				},
				{
					id: 4,
					show: false
				}
			],
			dropBalls: [],
			fold: true
		};
	},
	methods: {
		drop(el){
			for (let i = 0; i < this.balls.length; i++) {
				let ball = this.balls[i];
				if (!ball.show) {
					ball.show = true;
					ball.el = el;
					this.dropBalls.push(ball);
					return;
				}
			}
		},
		toggle() {
			if (this.totalCount) {
				console.log('here');
				this.fold = !this.fold;
			}
		}
	},
	transitions: {
		drop: {
			beforeEnter(el) {
				for (let i = 0; i < this.balls.length; i++) {
					let ball = this.balls[i];
					if (ball.show) {
						let rect = ball.el.getBoundingClientRect();
						let x = rect.left - 32;
						let y = -(window.innerHeight - rect.top - 22);
						el.style.display = 'block';
						el.style.webkitTransform = `translate3d(0,${y}px,0)`;
						el.style.transform = `translate3d(0,${y}px,0)`;
						let inner = el.getElementsByClassName('inner-hook')[0];
						inner.style.webkitTransform = `translate3d(${x}px,0,0)`;
						inner.style.transform = `translate3d(${x}px,0,0)`;
					}
				}
			},
			enter(el) {
				this.$nextTick(() => {
					el.offsetWidth;
					el.style.webkitTransform = 'translate3d(0,0,0)';
					el.style.transform = 'translate3d(0,0,0)';
					let inner = el.getElementsByClassName('inner-hook')[0];
					inner.style.webkitTransform = 'translate3d(0,0,0)';
					inner.style.transform = 'translate3d(0,0,0)';
				});
			},
			afterEnter(el) {
				let ball = this.dropBalls.shift();
				console.log('dropBalls: ', this.dropBalls, ball);
				if (ball) {
					console.log('shift ball', ball);
					ball.show = false;
					el.style.display = 'none';
				}
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
		},
		listShow() {
			if (!this.totalCount){ // totalCount == 0
				this.fold = true;
				return;
			}
			return !this.fold;
		}
	},
	components: {
		cartcontrol: cartcontrol
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
					display: inline-block
					border-width: 0
					outline-width: 0
					width: 100%
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
		.ball-container
			.ball
				position: fixed
				left: 32px
				bottom: 22px
				z-index: 200
				&.drop-transition
					transition: all .4s cubic-bezier(.49,-0.29,.75,.41)
					.inner
						width: 16px
						height: 16px
						background-color: rgb(0,160,220)
						border-radius: 50%	
						transition: all .4s	linear
		.shopcart-list
			position: fixed;
			top: 0;
</style>


