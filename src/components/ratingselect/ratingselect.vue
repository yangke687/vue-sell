<template>
	<div class="ratingSelect">
		<div class="rating-type">
			<span>{{desc.all}}</span>
			<span>{{desc.positive}}</span>
			<span>{{desc.negative}}</span>
		</div>
		<div class="switch">
			<span class="icon-check_circle"></span>
			<span>只看有内容的评价</span>
		</div>
	</div>
</template>

<script type="text/ecmascript-6">
	const POSITIVE = 0;
	const NEGATIVE = 1;
	console.log(POSITIVE, NEGATIVE);
	const ALL = 2;
	export default {
		props: {
			ratings: {
				type: Array,
				defaut() {
					return [];
				}
			},
			selectType: {
				type: Number,
				default: ALL
			},
			onlyContent: {
				type: Boolean,
				default: false
			},
			desc: {
				type: Object,
				default() {
					return {
						all: '全部',
						positive: '满意',
						negative: '不满意'
					};
				}
			}
		}
	};
</script>

<style lang="stylus" rel="stylesheet/stylus">
	
</style>