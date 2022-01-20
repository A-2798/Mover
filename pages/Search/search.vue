<template>
	<view class="search" v-if="cart.length !== 0">
		<my-addres class=""> </my-addres>
		<view class="cart-title">
			<!-- 右侧的图标 -->
			<uni-icons type="shop" size="18"></uni-icons>
			<!-- 右侧文本 -->
			<text class="cart-text">购物车</text>
		</view>
		<!-- 循环渲染购物车中的商品信息 -->
		<!-- uni-swipe-action 是最外层包裹性质的容器 -->
		<uni-swipe-action>
			<block v-for="(goods, i) in cart" :key="i">
				<uni-swipe-action-item  :right-options="options" @click="swipeActionClickHandler(goods)">
					<my-goods :goods="goods" :show-radio="true" :show-num="true" @radio-change="radioChangeHandler"
						@num-change="numberChangeHandler"></my-goods>
				</uni-swipe-action-item>
			</block>
		</uni-swipe-action>
		
		<!-- 结算区域 -->
		<my-settle></my-settle>
	</view>
	
	  <!-- 空白购物车区域 -->
	  <view class="empty-cart" v-else>
	    <image src="@/static/shopping.png" class="empty-img"></image>
	    <text class="tip-text">空空如也~</text>
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
			return {
				options: [{
					text: '删除', // 显示的文本内容
					style: {
						backgroundColor: '#C00000' // 按钮的背景颜色
					}
				}]
			}
		},
		onShow() {

		},
		methods: {
			...mapMutations('m_cart', ['updateGoodsState', 'updateGoodsCount', 'removeGoodsById']),
			// 商品的勾选状态发生了变化
			radioChangeHandler(e) {
				this.updateGoodsState(e)
			},
			// 商品的数量发生了变化
			numberChangeHandler(e) {
				this.updateGoodsCount(e)
			},
			// 点击了滑动操作按钮
			swipeActionClickHandler(goods) {
				 this.removeGoodsById(goods.goods_id)
			}
		}
	}
</script>

<style scoped lang="scss">
	.search{
		padding-bottom: 50px;
	}
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
	.empty-cart {
	  display: flex;
	  flex-direction: column;
	  align-items: center;
	  padding-top: 150px;
	
	  .empty-img {
	    width: 90px;
	    height: 90px;
	  }
	
	  .tip-text {
	    font-size: 12px;
	    color: gray;
	    margin-top: 15px;
	  }
	}
</style>
