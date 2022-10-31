<template>
	<view class="container">
		<u-toast ref="uToast" />
		<u-navbar back-icon-color='#333' title-bold title='找回密码' title-color='#333'></u-navbar>
		<u-gap height="20" bg-color="#F3F4F5"></u-gap>
		<view class="content">
			<u-form :model="form" ref="uForm">
				<u-form-item label="邮箱地址" prop='mail' label-position='top' :border-bottom='false'>
					<u-input v-model="form.mail" border :clearable='false' :custom-style='inputStyle'
						placeholder='请输入邮箱地址' />
					<view class="send-code" @click="countDown">
						{{countDownText}}
					</view>
				</u-form-item>
				<u-form-item label="邮箱验证码" prop='code' label-position='top' :border-bottom='false'>
					<u-input type='number' v-model="form.code" border :clearable='false' :custom-style='inputStyle2'
						placeholder='请输入邮箱验证码' />
				</u-form-item>
				<!-- 新密码 -->
				<view style="margin-top: 40rpx;">
					<view style="margin-bottom:20rpx; font-size:32rpx;">
						新密码
						<span style='margin-left:10rpx; font-size:28rpx; color:#999;'>
							(包含字母、数字 最少8位)
						</span>
					</view>
					<u-form-item label=" " prop='password' :label-style='{padding:"0"}' label-position='top'
						:border-bottom='false'>
						<u-input type='password' v-model="form.newPassWord" border :clearable='false'
							:custom-style='inputStyle2' :password-icon='false' placeholder='请输入新密码' />
					</u-form-item>
				</view>
				<!-- 确认新密码 -->
				<u-form-item label="确认新密码" prop='confirmPwd' label-position='top' :border-bottom='false'>
					<u-input type='password' v-model="form.confirmPwd" border :clearable='false'
						:custom-style='inputStyle2' :password-icon='false' placeholder='请确认新密码' />
				</u-form-item>
			</u-form>
			<button class="sub" @click="submit">提交</button>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				inputStyle: {
					padding: '0 210rpx 0 0',
					borderRadius: '16rpx'
				},
				inputStyle2: {
					borderRadius: '16rpx'
				},
				countDownText: '发送验证码',
				countDownTime: 60,
				isCountDown: true, //是否启动验证码的倒计时
				form: {
					mail: '',
					code: '',
					newPassWord: '',
					confirmPwd: '',
				},
				rules: {
					mail: [{
							required: true,
							message: '请填写邮箱',
							trigger: 'blur'
						},
						{
							pattern: /^\w+([-+.]\w+)*@\w+([-.]\w+)*\.\w+([-.]\w+)*$/g,
							// 正则检验前先将值转为字符串
							transform(value) {
								return String(value);
							},
							message: '请输入正确的邮箱',
							trigger: 'blur'
						}
					],
					code: [{
						required: true,
						message: '请填写验证码',
						trigger: 'blur'
					}],
					newPassWord: [{
						required: true,
						message: '请填写新密码',
						trigger: 'blur'
					}, {
						pattern: /^(?=.*[0-9].*)(?=.*[A-Za-z].*).{8,}$/g,
						// 正则检验前先将值转为字符串
						transform(value) {
							return String(value);
						},
						message: '新密码必须包含字母、数字 最少8位',
						trigger: 'blur'
					}],
					confirmPwd: [{
						required: true,
						message: '请确认密码是否一致',
						trigger: 'blur'
					}, {
						validator: (rule, value, callback) => {
							value = this.form.newPassWord == this.form.confirmPwd
							return value;
						},
						message: '两次密码输入不一致',
						trigger: 'blur'
					}]
				},
			}
		},
		methods: {
			countDown() {
				let mailReg = /^\w+([-+.]\w+)*@\w+([-.]\w+)*\.\w+([-.]\w+)*$/g
				let isMail = mailReg.test(this.form.mail)
				// 判断是否启动了定时器,并且邮箱是否正确
				if (this.isCountDown && isMail) {
					this.isCountDown = false
					let timer = setInterval(() => {
						this.countDownTime--
						this.countDownText = `${this.countDownTime}S`
						if (this.countDownTime <= 0) {
							this.countDownText = '发送验证码'
							this.countDownTime = 60
							this.isCountDown = true
							clearInterval(timer)
						}
					}, 1000)
					this.getCode()
				}
			},
			getCode() {
				this.$u.api.getcode({
					mail: this.form.mail,
					type: 1
				}).then(res => {

				})
			},
			submit() {
				this.$refs.uForm.validate(valid => {
					if (valid) {
						// delete this.form.confirmPwd
						console.log('验证通过');
						this.form.newPassWord = this.$md5(this.form.newPassWord +'uEe')
						this.form.confirmPwd = this.$md5(this.form.confirmPwd +'uEe')
						this.$u.api.forgotPWD(this.form).then(res => {
							if (res.code == 0) {
								this.$refs.uToast.show({
									title: '找回成功',
									type: 'success',
									back: true,
								})
							} else {
								this.$refs.uToast.show({
									title: '找回失败',
									type: 'error',
								})
							}
						})
					} else {
						this.$refs.uToast.show({
							title: '表单填写不正确',
							type: 'error',
						})
					}
				});
			}
		},
		onReady() {
			this.$refs.uForm.setRules(this.rules);
		}
	}
</script>

<style lang="scss" scoped>
	.container {
		min-height: 100vh;
		background: #fff;
	}

	::v-deep .u-form-item {
		padding: 0;
		margin-top: 30rpx;
	}

	.content {
		padding: 0 30rpx 50rpx;

		.send-code {
			position: absolute;
			top: 220rpx;
			right: 32rpx;
			padding: 0 20rpx;
			width: 200rpx;
			height: 50rpx;
			line-height: 50rpx;
			text-align: center;
			font-size: 28rpx;
			color: #237FF8;
			border-left: 1rpx solid #C8CFDE;
		}

		.sub {
			margin: 300rpx 0 60rpx 0;
			height: 96rpx;
			line-height: 96rpx;
			font-size: 32rpx;
			font-weight: bold;
			color: #fff;
			border-radius: 48rpx;
			background: #8C93A1;
		}
	}
</style>
