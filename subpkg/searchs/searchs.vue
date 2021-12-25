<template>
	<view>
		<view class="search-box">
			<!-- 基本用法 -->
			<uni-search-bar @confirm="search" :radius="100" @input="input" cancelButton="none" placeholder="请输入"></uni-search-bar>
		</view>

		<!-- 搜索建议列表 -->
		<view class="sugg-list" v-if="searchResults.length !== 0 ">
			<view class="sugg-item" v-for="(item,index) in searchResults" :key="index" @click="gotoDetail(item)">
				<view class="goods-name">{{item.goods_name}}</view>
				<uni-icons type="arrowright" size="16"></uni-icons>
			</view>
		</view>
		
		<!-- 搜索历史记录 -->
		<view class="historyList-box" v-else>
			<!-- 标题区域 -->
			<view class="historyList-title">
				<text>搜索历史</text>
				<uni-icons type="trash" size="17" @click="clean"></uni-icons>
			</view>
			<view class="historyList-list">
				<uni-tag class="tag" :text="item2" v-for="(item2,index2) in histories" :key="index2" @click="goodlist(item2)"></uni-tag>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				timer: null,
				kw: '',
				searchResults: [],
				historyList:[] //搜索历史记录
			};
		},
		onLoad() {
			this.historyList = JSON.parse(uni.getStorageSync('kw') || '[]')
		},
		methods: {
			// 防抖
			input(e) {
				clearTimeout(this.timer) // 清除防抖定时器

				this.timer = setTimeout(() => {
					this.kw = e
					console.log(e, '666')
					//根据关键词 查询搜索建议列表
					this.getSearchList()
				}, 500)
			},
			// 根据搜索关键词，搜索商品建议列表
			async getSearchList() {
				// 判断关键词是否为空
				if (this.kw === '') {
					this.searchResults = []
					return
				}
				// 发起请求，获取搜索建议列表
				const {
					data: res
				} = await uni.$http.get('/api/public/v1/goods/qsearch', {
					query: this.kw
				})
				if (res.meta.status !== 200) return uni.$showMsg()
				this.searchResults = res.message
				// 方法
				this.saveSearchHistory()
			},
			gotoDetail(item) {
				uni.navigateTo({
					url: '/subpkg/goods/goods?goods_id=' + item.goods_id
				})
			},
			saveSearchHistory(){ // 此时搜索历史会排在最后一项
				// this.historyList.push(this.kw)
				const set = new Set(this.historyList)
				set.delete(this.kw)
				set.add(this.kw)
				
				this.historyList = Array.from(set)
				//数据存储
				uni.setStorageSync('kw',JSON.stringify(this.historyList))
			},
			clean(){
				this.historyList = []
				uni.setStorageSync('kw','[]')
			},
			goodlist(kw){
				uni.navigateTo({
					url:'/subpkg/goods/goods?query=' + kw
				})
			}
		},
		computed: {
			//histories为复数的语法 reverse() 方法反转数组中元素的顺序。
			histories(){
			return	[...this.historyList].reverse()
			}
		},
	}
</script>

<style lang="scss" scoped>
	.search-box {
		position: sticky; //吸顶效果
		top: 0;
		z-index: 999;
		background-color: #C00000;
	}

	.sugg-list {
		padding: 0 5px;

		.sugg-item {
			display: flex;
			align-items: center;
			justify-content: space-between;
			font-size: 12px;
			padding: 13px 0;
			border-bottom: 1px solid #efefef;

			.goods-name {
				white-space: nowrap;
				overflow: hidden;
				text-overflow: ellipsis;
			}
		}
	}
	.historyList-box{
		padding: 0 5px;
		.historyList-title{
			display: flex;
			justify-content: space-between;
			align-items: center;
			font-size: 13px;
			height: 40px;
			border-bottom: 1px solid #efefef;
		}
		.historyList-list{
			display: flex;
			flex-wrap: wrap;
			.tag /deep/ .uni-tag  {
				margin: 5px 10px 0 0;
			}
		}
	}
</style>
