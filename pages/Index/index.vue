<template>
	<view class="content">
		<!-- 搜索组件 -->
		<view class="search-box">
			<my-search @myclick="gotoSearch"></my-search>
		</view>
		<!-- 轮播图区域 -->
		<swiper :indicator-dots="true" :autoplay="true" :interval="3000" :duration="1000" :circular="true">
			<swiper-item class="swiper" v-for="(item,index) in swipers" :key="index">
				<navigator class="swiper-item" :url="'/subpkg/goods/goods?goods_id=' + item.goods_id">
					<image :src="item.image_src"></image>
				</navigator>
			</swiper-item>
		</swiper>
		<!-- 分类导航区域 -->
		<view class="nav-list">
			<view class="nav-item" v-for="(item,index) in navlist" :key="index" @click="tapNav(item)">
				<image class="nav-img" :src="item.image_src"></image>
			</view> 
		</view>
		<!-- 获取楼层数据 -->
		<view class="foollist_list">
			<view class="fool-item" v-for="(item,index) in foollist" :key="index">
				<image class="fool-title"  :src="item.floor_title.image_src"></image>
				<view class="fool-img-box">
					<navigator class="fool-left-box" :url="item.product_list[0].url">
						<image :src="item.product_list[0].image_src" :style="{width:item.product_list[0].image_width + 'rpx'} " mode="widthFix"></image>
					</navigator>
					<view class="fool-right-box">
						<navigator class="right-img-box" v-for="(item2,index2) in item.product_list" :key="index2" v-if="index2 !== 0" :url="item2.url">
							<image :src="item2.image_src" :style="{width:item2.image_width + 'rpx'} " mode="widthFix"></image>
						</navigator>
					</view>
				</view>
			</view>
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
				swipers: [], //轮播图
				navlist: [], //nav的导航栏
				foollist: [] //获取楼层数据
			}
		},
		onLoad() {
			this.swiper()
			this.getnavlist()
			this.getfoollist()
		},
		methods: {
			//轮播图数据请求
			async swiper() {
				const {
					data: res
				} = await uni.$http.get('/api/public/v1/home/swiperdata')
				console.log(res, '222')
				if (res.meta.status !== 200)
					return uni.showMsg()
				this.swipers = res.message
				return uni.showMsg('数据请求成功!')
			},
			//nav的导航栏数据请求
			async getnavlist() {
				const {
					data: res
				} = await uni.$http.get('/api/public/v1/home/catitems')
				if (res.meta.status !== 200)
					return uni.showMsg()
				this.navlist = res.message
				return uni.showMsg('数据请求成功!')
			},
			tapNav(item) {
				console.log(item, '111')
				if (item.name === '分类') {
					uni.switchTab({
						url: '/pages/Classify/classify'
					})
				}
			},
			 //获取楼层数据请求
			async getfoollist() {
				const {
					data: res
				} = await uni.$http.get('/api/public/v1/home/floordata')
				if (res.meta.status !== 200)
					return uni.showMsg()
					//对数据进行处理
					res.message.forEach(floor => {
						floor.product_list.forEach(prod =>{
							prod.url = '/subpkg/goods/goods?' + prod.navigator_url.split('?')[1]
							// split() 方法用于把一个字符串分割成字符串数组。
						})
					})
				this.foollist = res.message
				return uni.showMsg('数据请求成功!')
			},
			gotoSearch(){
				uni.navigateTo({
					url:'/subpkg/searchs/searchs'
				})
			}
		}
	}
</script>

<style scoped lang="scss">
	@import "../../pages/Style.scss";
</style>
