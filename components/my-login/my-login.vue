<template>
	<view class="login-container">
		<uni-icons type="contact-filled" size="100" color="#AFAFAF"></uni-icons>
		<button type="default" class="btn-login" open-type="getUserInfo" @getuserinfo="getUserInfo">
			<!-- 固定写法用于拿到用户的信息open-type="getUserInfo"  @getuserInfo="getUserInfo"-->
			<text>一键登录</text>
		</button>
		<text class="tips-text">登录后尽享更多权益</text>
	</view>
</template>

<script>
	// 1. 按需导入 mapMutations 辅助函数
	import {
		mapMutations
	} from 'vuex'
	export default {
		data() {
			return {

			}
		},
		onLoad() {

		},
		methods: {
			  // 1. 使用 mapMutations 辅助方法，把 m_user 模块中的 updateToken 方法映射到当前组件中使用
			  ...mapMutations('m_user', ['updateUserInfo', 'updateToken']),
			//用户授权之后获取用户基本信息
			getUserInfo(e) {
				console.log(e)
				if (e.detail.errMsg === 'getUserInfo:fail auth deny') return uni.showMsg('您取消了授权!')
				console.log(e.detail.userInfo, '666')

				// 3. 将用户的基本信息存储到 vuex 中
				this.updateUserInfo(e.detail.userInfo)
				
				 // 获取登录成功后的 Token 字符串
				  this.getToken(e.detail)
			},
			// 调用登录接口，换取永久的 token
			async getToken(info) {
			  // 调用微信登录接口
			  const [err, res] = await uni.login().catch(err => err)
			  // 判断是否 uni.login() 调用失败
			  if (err || res.errMsg !== 'login:ok') return uni.showError('登录失败！')
				
			  // 准备参数对象
			  const query = {
			    code: res.code,
			    encryptedData: info.encryptedData,
			    iv: info.iv,
			    rawData: info.rawData,
			    signature: info.signature
			  }
			
			  // 换取 token
			  const { data: loginResult } = await uni.$http.post('/api/public/v1/users/wxlogin', query)
			  if (loginResult.meta.status !== 200) return uni.showMsg('登录失败！')
			  
			     // 2. 更新 vuex 中的 token
			      this.updateToken(loginResult.message.token)
			}
		}
	}
</script>

<style lang="scss" scoped>
	.login-container {
		height: 750rpx;
		background-color: #F8F8F8;
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		overflow: hidden;
		position: relative;

		&::after {
			position: absolute;
			content: ' ';
			display: block;
			width: 100%;
			height: 40px;
			background: white;
			bottom: 0;
			left: 0;
			border-radius: 100%;
			transform: translateY(50%);

		}

		.btn-login {
			width: 90%;
			border-radius: 100px;
			margin: 15px 0;
			background-color: #C00000;
		}

		.tips-text {
			font-size: 12px;
			color: gray;
		}
	}
</style>
