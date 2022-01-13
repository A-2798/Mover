<template>
	<view class="contnt">
		<view class="uniui-paperclip">
		211333333333333333333333333333333333333333333333333333333333333333333333333333333333333333333333333333333333333333333333333333333333333335666666666666666666666666666666666666666666666666666666666666666666666666666666667
		</view>
	</view>
</template>

<script>
// 导入自己封装的 mixin 模块
	import badgeMix from '@/mixins/tabbar-badge.js'
	export default {
	  // 将 badgeMix 混入到当前的页面中进行使用
	  mixins: [badgeMix],
		data() {
			return {
				
			}
		},
		onLoad() {

		},
		methods: {

		}
	}
</script>

<style scoped lang="scss">
	.contnt{
		background: red;
		width: 200px;
		height: 200px;
		min-width: 220px;
		.uniui-paperclip{
		
			background: yellow;
		}
	}
	
</style>
