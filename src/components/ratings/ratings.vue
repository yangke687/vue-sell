<template>
	<div class="ratings">
		<div class="ratings-content">
			<div class="overview">
				<div class="overview-left">
					<h1 class="score">{{seller.score}}</h1>
					<div class="title">综合评分</div>
					<div class="rank">高于周边商家{{seller.rankRate}}%</div>
				</div>
				<div class="overview-right">
					<div class="score-wrapper">
						<span class="title">服务态度</span>
						<star :score="seller.serviceScore" :size="36"></star>
						<span class="score">{{seller.serviceScore}}</span>
					</div>
					<div class="score-wrapper">
						<span class="title">商品评分</span>
						<star :score="seller.foodScore" :size="36"></star>
						<span class="score">{{seller.foodScore}}</span>
					</div>
					<div class="delivery-time">
						<span class="title">送达时间</span>
						<span class="delivery">{{seller.deliveryTime}}分钟</span>
					</div>
				</div>
			</div>
		</div>
	</div>
</template>

<script type="text/ecmascript-6">
	import star from 'components/star/star.vue';
	export default {
		props: {
			seller: {
				type: Object
			}
		},
		components: {
			star: star
		}
	};
</script>

<style lang="stylus" rel="stylesheet/stylus">
	.ratings
		position: absolute
		top: 174px
		bottom: 0
		left: 0
		right: 0
		overflow: hidden
		.overview
			display: flex
			padding: 18px 0
			.overview-left
				flex: 0 0 137px
				padding: 6px 0
				border-right: 1px solid rgba(7,17,27,.1)
				text-align: center
				.score
					margin-bottom: 6px
					line-height: 28px
					color: rgb(255,153,0)
					font-size: 24px
				.title
					margin-bottom: 8px
					line-height: 12px
					font-size: 12px
					color: rgb(7,17,27)
				.rank
					line-height: 10px
					font-size: 10px
					color: rgb(147,153,159)
			.overview-right
				flex: 1
				padding: 6px 0 6px 24px
				.score-wrapper
					line-height: 18px
					margin-bottom: 8px
					font-size: 0
					.title
						display: inline-block
						vertical-align: top
						font-size: 12px
						color: rgb(7,17,27)
						line-height: 18px
					.star
						display: inline-block
						margin: 0 12px
						vertical-align: top
					.score
						display: inline-block
						vertical-align: top
						font-size: 12px
						line-height: 18px
						color: rgb(255,153,0)
				.delivery-time
					font-size: 0
					.title
						font-size: 12px
						color: rgb(7,17,27)
						line-height: 18px
					.delivery
						font-size: 12px
						color: rgb(147,153,159)
						padding-left: 12px
</style>