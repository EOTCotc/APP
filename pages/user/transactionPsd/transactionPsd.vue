<template>
	<view class="box">
		<u-navbar :is-back="true" :title="homeTitle" :background="background" :title-color="titleColor"
			:custom-back="about"></u-navbar>
		<u-form :model="form" ref="form1" class="main">
			<u-form-item label="" prop="mobile">
				<u-input v-model="form.mobile" disabled />
			</u-form-item>
			<u-form-item label="" prop="code" class="_code">
				<u-input v-model="form.code" placeholder="请输入验证码" @blur="input" :disabled="disabled" />
				<button class="custom-style" size="mini" slot="right" @click="getCode" ref="code">{{codeText}}</button>
			</u-form-item>
			<u-verification-code ref="uCode" @change="codeChange" @end="end" @start="start"></u-verification-code>
			<view class="ht">设置密码</view>
			<u-form-item label="" prop="password">
				<u-input v-model="form.password" placeholder="输入密码" :type="type1" @blur="input"
					:password-icon="passwordIcon1" />
			</u-form-item>
			<u-form-item label="" prop="Tpassword">
				<u-input v-model="form.Tpassword" placeholder="输入密码" :type="type2" @blur="input"
					:password-icon="passwordIcon2" />
			</u-form-item>
		</u-form>
		<view class="footer" v-if="flag">
			<u-button shape="circle" :custom-style="customStyle" :disabled="flag" :hair-line="false">确认</u-button>
		</view>
		<view class="footer" v-if="flag==false">
			<u-button shape="circle" style="background: linear-gradient(to right,#1758F1, #37DBE0)" :loading="sub"
				:hair-line="false" @click="submit">确认</u-button>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				background: {
					backgroundColor: '#FFFFFF'
				},
				customStyle: {
					backgroundColor: '#D8D8D8'
				},
				type1: 'password',
				passwordIcon1: true,
				type2: 'password',
				passwordIcon2: true,
				titleColor: '#333333',
				homeTitle: '交易密码',
				// localStorage.getItem(user.mail) || 
				form: {
					mobile: '2356535286@qq.com',
					code: '',
					password: '',
					Tpassword: '',
				},
				disabled: true,
				timer:null,
				// resultCode: '123456',
				codeText: '',
				flag: true,
				sub: false,
				op1: false,
				op2: false,
				op3: false,
				timer: null,
				rules: {
					code: [{
						type: 'string',
						required: true,
						len: 6,
						message: '不可为空,请填写6位验证码',
						// 可以单个或者同时写两个触发验证方式
						trigger: ["change", "blur"]
					 }, 
					// {
					// 	validator: (rule, value, callback) => {
					// 		let Reg = new RegExp(this.resultCode, "g")
					// 		// 上面有说，返回true表示校验通过，返回false表示不通过
					// 		// this.$u.test.mobile()就是返回true或者false的
					// 		if (!Reg.test(value.trim())) {
					// 			this.op1 = false
					// 			callback('验证码输入错误')
					// 		} else {
					// 			callback()
					// 			this.op1 = true
					// 		}
					// 	},
					// 	// 触发器可以同时用blur和change
					// 	trigger: ['change', 'blur'],
					// },
					],
					password: [{
							type: 'string',
							min: 6,
							max: 10,
							required: true,
							message: '不可为空,长度在6-8个字符之间',
							trigger: ['blur', 'change']
						},
						// 正则判断为字母或数字
						{
							validator: (rules, value, callback) => {
								let Reg = (/^[0-9a-zA-Z]*$/g)
								if (!Reg.test(String(value).trim())) {
									callback('只能包含字母或数字')
									this.op2 = false
								} else {
									callback()
									this.op2 = true
								}
							},
							trigger: ['blur', 'change']
						},

					],
					Tpassword: [{
						type: 'string',
						required: true,
						message: '密码不为空',
						trigger: ['blur', 'change']
					}, {
						validator: (rules, value, callback) => {
							if (value != this.form.password) {
								callback(new Error('两次密码校验不一致'))
								this.op3 = false
							} else {
								callback()
								this.op3 = true
							}
						},
						trigger: ["change", "blur"]
					}]
				},
			};
		},
		onReady() {
			clearTimeout(this.timer)
			this.$refs.form1.setRules(this.rules)
		},

		methods: {
			about() {
				uni.reLaunch({
					url: '/pages/user/index'
				})
			},
			codeChange(text) {
				this.codeText = text;
				console.log(text)
			},
			getCode() {
				if (this.$refs.uCode.canGetCode) {
					// 模拟向后端请求验证码
					uni.showLoading({
						title: '正在获取验证码'
					})
					this.disabled = false
					this.$u.api.getcode({mail:this.form.mobile,type:1}).then((res)=>{
						console.log(res)
					})
					this.timer=setTimeout(() => {
						uni.hideLoading();
						// 通知验证码组件内部开始倒计时
						this.$refs.uCode.start();
					}, 1000);
					
				} else {
					this.$u.toast('倒计时结束后再发送');
				}
			},
			end() {
				this.$u.toast('倒计时结束');
				console.log(this.code,'this.code')
				clearTimeout(this.timer)
				if(this.code==undefined || this.code=='')this.disabled = true
			},
			start() {
				this.$u.toast('倒计时开始');
				this.disabled = false
			},
			input(val) {
				console.log(this.op1, this.op3, this.op2)
				if (this.op3 == true && this.op2 == true) {
					this.flag = false
				} else {
					this.flag = true
				}
				// console.log(this.$refs.form1.validate(),1111)
			},
			submit() {
				this.sub = true
				clearTimeout(this.timer)
				this.$refs.form1.validate(valid => {
					if (valid) {
						console.log('验证通过');
						
						this.$u.api.setPayPassword({mail:this.form.mobile,payPassWord:this.$md5(this.form.Tpassword),code:this.form.code}).then((res)=>{
							console.log(res)
							uni.showLoading({
								title: '设置成功'
							})
							this.timer = setTimeout(() => {
								this.sub = false
								uni.reLaunch({
									url: '/pages/user/index'
								})
								uni.hideLoading();
							}, 1000)
						})
					} else {
						console.log('验证失败');
						uni.showLoading({
							title: '设置失败'
						})
						setTimeout(() => {
							this.sub = false
							uni.hideLoading();
						}, 1000)
					}
				});


			}
		}
	}
</script>

<style lang="scss" scoped>
	.u-form {
		background-color: #fff;
	}

	.custom-style {
		color: #1758F1 !important;
		font-size: 26rpx !important;
		background-color: #fff;
	}

	uni-button:after {
		border: none !important;
	}

	.u-form-item {
		padding-right: 30rpx;
		padding-left: 30rpx;
	}

	.ht {
		font-size: 32rpx;
		color: #333333;
		padding: 30rpx;
		background-color: #F5F6F7;

	}

	::v-deep ._code .uni-input-input {
		box-sizing: border-box;
		border-right: 1px solid #F5F6F7;
	}

	.main {
		margin-top: 16rpx;
	}

	::v-deep .u-btn--default[data-v-6e15e680] {
		color: #fff;
	}

	::v-deep .u-form-item__message[data-v-006449ec] {
		padding-left: 0 !important;
		}
	.footer {
		padding: 0 20rpx;
		width: 100%;
		position: absolute;
		bottom: 40rpx;
		left: 0;
	}

	::v-deep .u-field-inner[data-v-1c764f86] {
		display: block;
	}
</style>
