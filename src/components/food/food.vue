<template>
	<div v-show="showFlag" class="food" transition="move">
		<div class="food-content">
			<div class="image-cotnent">
				<img :src="food.img" />
			</div>
		</div>
	</div>
</template>

<script type="text/ecmascript-6">
	export default{
		props: {
			food: {
				type: Object
			}
		},
		data() {
			return {
				showFlag: false
			};
		},
		methods: {
			show() {
				this.showFlag = true;
			}
		}
	};
</script>

<style lang="stylus" rel="stylesheet/stylus">
	.food
		position: fixed
		left: 0
		right: 0
		top: 0
		bottom: 48px
		z-index: 30
		background: #FFF
		transition: all .2s linear
		&.move-transition
			transform: translate3d(0,0,0)
		&.move-enter,&.move-leave
			transform: translate3d(100%,0,0)
</style>