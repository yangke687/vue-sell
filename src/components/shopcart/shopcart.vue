<template>
	<div class="shopcart">
		<div class="content">
			<div class="content-left">
				<div class="logo-wrapper">
					<div class="logo">
						<span class="icon-shopping_cart"></span>
					</div>
				</div>
				<div class="price">¥ 10</div>
				<div class="desc">另需配送费 ¥ {{ deliveryPrice }}元</div>
			</div>
			<div class="content-right">
				<div class="pay">¥{{ minPrice }}元起配送</div>
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
						span
							line-height: 44px
						    font-size: 24px
						    color: #80858a
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

</style>


