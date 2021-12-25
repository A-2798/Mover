<template>
	<view class="goods-item">
		<!-- 左侧盒子 -->
		<view class="goods-item-left">
			<image class="goods-pic" :src="goods.goods_small_logo || defaultPic" ></image>
		</view>
		<!-- 右侧盒子 -->
		<view class="goods-item-right">
			<!-- 商品名字 -->
			<view class="goods-name">{{goods.goods_name}}</view>
			<view class="goods-info-box">
				<view class="goods-price">￥{{goods.goods_price | tofixed}}</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		props:{
			goods:{
				type:Object,
				default:{}
			}
		},
		data() {
			return {
				defaultPic: 'https://img3.doubanio.com/f/movie/8dd0c794499fe925ae2ae89ee30cd225750457b4/pics/movie/celebrity-default-medium.png'
			};
		},
		filters:{ //过滤器来处理价格 2位数
			tofixed(num){
				return Number(num).toFixed(2)
			}
		}
	}
</script>

<style scoped lang="scss">
	.goods-item{
		display: flex;
		padding: 10px 5px;
		border-bottom: 1px solid #F0F0F0;
		.goods-item-left{
			margin-right: 5px;
			.goods-pic{
				width: 100px;
				height: 100px;
				display: block;
			}
		}
		.goods-item-right{
			display: flex;
			justify-content: space-between;
			flex-direction: column;
			.goods-name{
				font-size: 13px;
			}
			.goods-info-box{
				.goods-price{
					color: red;
				}
			}
		}
	}
</style>
