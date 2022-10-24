<template>
	<view class="container">
		<u-toast ref="uToast" />
		<view class="top">
			<view>
				<img src="@/static/images/buy/logo.png">
			</view>
			<view class="app-name">EOTC</view>
		</view>
		<view class="content">
			<view class="title">登录</view>
			<view>
				<u-form :model="form" ref="uForm" label-position='top'>
					<u-form-item label="邮箱地址" label-position='top' :border-bottom='false'>
						<u-input v-model="form.mail" :clearable='false' :custom-style="inputStyle1"
							placeholder='请输入邮箱地址' />
						<view class="send-code">
							<u-button size='mini' @click='getCode' :custom-style='btnStyle' hover-class='none'>
								{{codeBtnText}}
							</u-button>
						</view>
					</u-form-item>
					<u-form-item label="邮箱验证码" label-position='top' :border-bottom='false'>
						<u-input v-model="form.code" :clearable='false' :custom-style="inputStyle2"
							placeholder='请输入邮箱验证码' />
					</u-form-item>
				</u-form>
			</view>
			<button class="login-btn" @click="login">登录</button>
			<navigator url="/pages/login/forgotPwd">
				<view class="forget">忘记密码</view>
			</navigator>

		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				inputStyle1: {
					padding: '0 180rpx 0 30rpx',
					border: '1px solid #C8CFDE',
					borderRadius: '16rpx'
				},
				inputStyle2: {
					padding: '0 30rpx',
					border: '1px solid #C8CFDE',
					borderRadius: '16rpx'
				},
				btnStyle: {
					width: '160rpx',
					height: '72rpx',
					border: 'none',
					background: '#1B2945',
					color: '#fff',
				},
				codeBtnText: '发送验证码',
				codeBtnNum: 60,
				isCodeBtn: true,
				form: {
					mail: '',
					code: '',
				}
			}
		},
		mounted() {
			let mymap = new Map()
			mymap.set(0, 'zero')
			mymap.set(1, 'one')
			mymap.set('fun', function() {
				return 123;
			})
			console.log(mymap.length);
		},
		methods: {
			// 获取验证码
			getCode() {
				if (this.form.mail && this.isCodeBtn) {
					this.isCodeBtn = false //倒计时禁用按钮
					this.btnStyle.background = '#8C93A1'
					let timer = setInterval(() => {
						this.codeBtnNum--
						this.codeBtnText = `${this.codeBtnNum}S`
						if (this.codeBtnNum <= 0) {
							this.isCodeBtn = true
							this.codeBtnNum = 60
							this.btnStyle.background = '#1B2945'
							this.codeBtnText = '发送验证码'
							clearInterval(timer)
						}
					}, 1000)
					this.$u.api.getcode({
						mail: this.form.mail,
						type: 1
					}).then(res => {
						console.log(res);
					})
				}
			},
			// 登录
			login() {
				this.$u.api.login(this.form).then(res => {
					console.log(res);
					if (res.code == 0) {
						this.$refs.uToast.show({
							title: '登录成功',
							type: 'success',
							icon: false,
							url: 'pages/buy/index',
							isTab: true,
						})
					} else {
						this.$refs.uToast.show({
							title: res.message,
							type: 'error',
							icon: false,
						})
					}
				})
			}
		}
	}
</script>

<style lang="scss" scoped>
	.container {
		padding: 0 32rpx;
		min-height: 100vh;
		display: flex;
		flex-direction: column;
		background: #1B2945;
		overflow: hidden;
	}

	.top {
		margin-top: 60rpx;
		text-align: center;

		img {
			width: 121rpx;
			height: 121rpx;
		}

		.app-name {
			margin-top: 20rpx;
			font-size: 38rpx;
			font-weight: bold;
			color: #E2ECFF;
		}
	}

	.content {
		position: relative;
		margin-top: 100rpx;
		padding: 48rpx 38rpx;
		flex: 1;
		background: #fff;
		border-top-left-radius: 20rpx;
		border-top-right-radius: 20rpx;

		.title {
			position: absolute;
			top: -48rpx;
			left: 0;
			width: 168rpx;
			height: 96rpx;
			line-height: 96rpx;
			text-align: center;
			font-size: 40rpx;
			font-weight: bold;
			color: #fff;
			border-radius: 48rpx;
			background: #1B2945;
			border: 4rpx solid #fff;
		}

		.send-code {
			position: absolute;
			top: 150rpx;
			right: 40rpx;
		}

		.login-btn {
			margin-top: 120rpx;
			height: 96rpx;
			line-height: 96rpx;
			font-size: 32rpx;
			color: #fff;
			border-radius: 48rpx;
			background: #1B2945;
		}

		.forget {
			margin-top: 40rpx;
			font-size: 28rpx;
			color: #999;
		}
	}
</style>
