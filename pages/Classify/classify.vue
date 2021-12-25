<template>
	<view>
		<!-- 自定义组件 之后 可以动态更改组件中的bgcolor radius 如果没有会执行组件中的配置 -->
		<my-search :bgcolor=" '#C00000' " :radius="30" @myclick="searchBox"></my-search>
		
		<view class="scroll-view-container">
			<!-- 左侧滑动区域 -->
			<scroll-view class="left-scroll-view" scroll-y="true" :style="{ height:wh + 'px' }">
				<block v-for="(item,index) in cateList" :key="index">
					<view :class="['left-scroll-view-item', index === active ? 'active' : '']"
						@click="activeChang(index)">{{item.cat_name}}</view>
				</block>
			</scroll-view>
			<!-- 右侧滑动区域 -->
			<scroll-view class="right-scroll-view" scroll-y="true" :style="{ height:wh + 'px' }">
				<view class="cate-l2" v-for="(item2,index2) in cateLevel2" :key="index2">
					<!-- 二级分类标题 -->
					<view class="cate-l2-title">/{{item2.cat_name}}/</view>
					<!-- 当前二级分类下的三级分类列表 -->
					<view class="cate-l3-list">
						<view class="cate-l3-item" v-for="(item3,index3) in item2.children" :key="index3"
							@click="gotolist(item3)">
							<image :src="item3.cat_icon" mode=""></image>
							<text>{{item3.cat_name}}</text>
						</view>
					</view>
				</view>
			</scroll-view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				wh: 0, //动态获取当前屏幕可用高度
				cateList: [], // 获取数据
				active: 0,
				cateLevel2: [] // 为二级菜单赋值
			};
		},
		onLoad() {
			// 动太获取屏幕高度
			const sysInfo = uni.getSystemInfoSync()
			console.log(sysInfo)
			this.wh = sysInfo.windowHeight - 50

			// 动态获取分类列表数据
			this.getCateList()
		},
		methods: {
			// 动态获取分类列表数据
			async getCateList() {
				const {
					data: res
				} = await uni.$http.get('/api/public/v1/categories')
				if (res.meta.status !== 200)
					return uni.showMsg()
				console.log(res, '3333')
				this.cateList = res.message
				return uni.showMsg('数据请求成功!')

				// 为二级菜单赋值
				this.cateLevel2 = res.message[0].children
			},

			activeChang(index) {
				this.active = index
				console.log(index, '666')
				this.cateLevel2 = this.cateList[index].children
			},

			// 跳转到商品列表页面
			gotolist(item) {
				uni.navigateTo({
					url: '/subpkg/goods/goods?cid=' + item.cat_id
				})
			},
			searchBox(){
				uni.navigateTo({
					url:'/subpkg/searchs/searchs'
				})
			},
			
		}
	} 
</script>

<style scoped lang="scss">
	.scroll-view-container {
		display: flex;

		.left-scroll-view {
			width: 120px;
		}
	}

	.left-scroll-view-item {
		background-color: #F7F7F7;
		font-size: 12px;
		line-height: 60px;
		text-align: center;

		&.active {
			background-color: #FFFFFF;
			position: relative;

			&::before {
				content: ' ';
				display: block;
				width: 3px;
				height: 30px;
				background-color: #C00000;
				position: absolute;
				top: 50%;
				left: 0;
				transform: translateY(-50%); //往撤回50%
			}
		}
	}

	.cate-l2-title {
		font-size: 12px;
		font-weight: bold;
		text-align: center;
		padding: 15px 0;
	}

	.cate-l3-list {
		display: flex;
		flex-wrap: wrap;

		.cate-l3-item {
			width: 33.33%;
			display: flex;
			justify-content: center;
			flex-direction: column;
			align-items: center;
			margin-bottom: 10px;

			image {
				width: 60px;
				height: 60px;
			}

			text {
				font-size: 12px;
			}
		}
	}
</style>
