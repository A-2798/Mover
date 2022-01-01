<template>
	<view>
		<view class="cart-title">
			<!-- 右侧的图标 -->
			<uni-icons type="shop" size="18"></uni-icons>
			<!-- 右侧文本 -->
			<text class="cart-text">购物车</text>
		</view>
		<!-- 循环渲染购物车中的商品信息 -->
		<block v-for="(goods, i) in cart" :key="i">
			<my-goods :goods="goods" :show-radio="true" :show-num="true" @radio-change="radioChangeHandler"
				@num-change="numberChangeHandler"></my-goods>
		</block>
	</view>
</template>

<script>
	// 导入自己封装的 mixin 模块
	import badgeMix from '@/mixins/tabbar-badge.js'
	import {
		mapState,
		mapMutations
	} from 'vuex'
	export default {
		// 将 badgeMix 混入到当前的页面中进行使用
		mixins: [badgeMix],
		computed: {
			...mapState('m_cart', ['cart'])
		},
		data() {
			return {}
		},
		onShow() {

		},
		methods: {
			...mapMutations('m_cart', ['updateGoodsState', 'updateGoodsCount']),
			// 商品的勾选状态发生了变化
			radioChangeHandler(e) {
				this.updateGoodsState(e)
			},
			// 商品的数量发生了变化
			numberChangeHandler(e) {
				this.updateGoodsCount(e)
			}
		}
	}
</script>

<style scoped lang="scss">
	.cart-title {
		display: flex;
		align-items: center;
		height: 40px;
		padding-left: 5px;
		border-bottom: 1px solid #EFEFEF;

		.cart-text {
			font-size: 14px;
			margin-left: 10px;
		}
	}
</style>
