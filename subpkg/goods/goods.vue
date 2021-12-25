<template>
	<view>
		<view class="goods-list">
			<view v-for="(goods,i) in goodsList" :key="i" @tap="gotoLetail(goods)">
				<my-goods :goods='goods'></my-goods>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				// 请求参数对象
				queryObj: {
					query: '',
					cid: '',
					pagenum: 1,
					pagesize: '10'
				},
				goodsList: [],
				total: 0,
				isloading:false //定义节流阀
			};
		},
		onLoad(options) {
			console.log(options, '669')
			this.queryObj.query = options.query || ''
			this.queryObj.cid = options.cid || ''
			console.log(this.queryObj, '555')

			this.getGoodsList()
		},
		methods: {
			// 获取商品列表数据的方法
			async getGoodsList(cd) {
				this.isloading = true //打开流阀
				const {
					data: res
				} = await uni.$http.get('/api/public/v1/goods/search', this.queryObj)
				this.isloading = false //关闭流阀
				cd && cd()
				if (res.meta.status !== 200)
					return uni.showMsg()
				this.goodsList = [...this.goodsList, ...res.message.goods] //添加下拉触底事件 用于加载页数 老数据加新数据
				this.total = res.message.total
				return uni.showMsg('数据请求成功!')
				console.log( res.message, '00000')
			},
			gotoLetail(goods){
				uni.navigateTo({
					url:'/subpkg/goods_list/goods_list?goods_id=' + goods.goods_id
				})
			}
		},
		onReachBottom(){ //添加下拉触底事件 用于加载页数 页面滚动到底部的事件（不是scroll-view滚到底）,常用于下拉下一页数据。
		if(this.queryObj.pagenum * this.queryObj.pagesize >= this.total) return uni.$showMsg('数据加载完成') 	//判断数据是否加载完毕
		    if(this.isloading) return //设置节流阀目的是防止 频繁获取页面数据请求问题
			//让页码自增+1
			this.queryObj.pagenum+=1
			this.getGoodsList()
		},
		onPullDownRefresh() { //监听该页面用户下拉刷新事件
			//重置关键数据
			this.queryObj.pagenum =1
			this.total = 0
			this.isloading = false
			this.goodsList = []
			//重新发起数据请求
			this.getGoodsList(() => uni.stopPullDownRefresh()) //停止当前页面下拉刷新
		}
	}
</script>

<style scoped lang="scss">
	
</style>
