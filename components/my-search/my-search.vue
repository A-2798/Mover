<template>
	<view class="my-search-container" :style="{'background-color': bgcolor }" @tap="searchBoxHander">
		<view class="my-search-box" :style="{'border-radius': radius + 'px' }">
			<uni-icons type="search" size="20"></uni-icons>
			<text class="placecholder">搜索</text>
		</view>
	</view>
</template>

<script>
	export default {
		props: {
			bgcolor: {
				type: String,
				default: '#C00000'
			},
			radius: {
				type: Number,
				default: 15 //px
			}
		},
		data() {
			return {

			};
		},
		methods:{
			searchBoxHander(){
				console.log(11)
				this.$emit('myclick')
			}
		}
	}
</script>

<style scoped lang="scss">
	@import "@/components/uni-icons/uniicons.css";

	.my-search-container {
		// background-color: #C00000;
		height: 50px;
		display: flex;
		align-items: center;
		padding: 0 10px;
		
		.my-search-box {
			height: 36px;
			background-color: #FFFFFF;
			// border-radius: 18px;
			width: 100%;
			display: flex;
			align-items: center;
			justify-content: center;

			.placecholder {
				margin-left: 5px;
				font-size: 15px;
			}
		}
	}
</style>
