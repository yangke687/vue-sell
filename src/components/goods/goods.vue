<template>
<div class="goods">
	<div class="menu-wrapper">
		<ul>
			<li v-for="item in goods" class="menu-item">
				<span class="text border-1px" >
				<span v-show="item.type>0" class="icon" :class="classMap[item.type]"></span>{{item.name}}
				</span>
			</li>
		</ul>
	</div>
	<div class="foods-wrapper">
		
	</div>
</div>
</template>
<script type="text/ecmascript-6">
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
					console.log(this.goods);
				}
			});
			this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
		},
		data(){
			return {
				goods: []
			};
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
		
</style>