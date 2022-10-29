<template>
	<view>
		<u-toast ref="uToast" />
		<u-no-network></u-no-network>
		<u-navbar title-bold :border-bottom='false' title='提交订单' title-color='#333'
			:background='{background:"#F5F6F7"}' :custom-back="about">
		</u-navbar>
		<view class="content">
			<!-- 购买的东西 -->
			<view class="card">
				<view><img src="@/static/images/buy/index_buy.png"></view>
				<view class="card-detail">
					<view class="card-title">币秋缠论指标系统</view>
					<view class="card-price" v-if="month==1">{{total}}U积分/月</view>
					<view class="card-price" v-if="month==12">{{total}}U积分/12月</view>
					<view class="number-box">
						<view class="minus" @click="num--" :style="num>1?'background:#FFBD7B;':''">-</view>
						<view class="num">{{num}}</view>
						<view class="add" @click="num++">+</view>
					</view>
				</view>
			</view>
			<!-- 个人信息 -->
			<view class="user-info">
				<view class="info-title">个人信息</view>
				<view class="info-form">
					<u-form :model="form" ref="uForm">
						<u-form-item required label="姓名" label-width='130' :label-style='{fontSize:"32rpx"}'>
							<u-input v-model="form.name" placeholder='请输入真实姓名' />
						</u-form-item>
						<u-form-item required label="手机号" label-width='130' :label-style='{fontSize:"32rpx"}'>
							<u-input type='number' v-model="form.phone" placeholder='请填写您的手机号码' />
						</u-form-item>
						<u-form-item required label="微信号" label-width='130' :border-bottom='false'
							:label-style='{fontSize:"32rpx"}'>
							<u-input v-model="form.wechat" placeholder='请填写您的微信号' />
						</u-form-item>
					</u-form>
				</view>
			</view>
			<!-- 服务说明 -->
			<view class="service">
				<view>服务说明</view>
				<view>支付成功后48小时内会有志愿者为您开通系统账号并联系您，请填写正确的个人信息。</view>
			</view>
		</view>
		<view class="sub">
			<view class="sub-l">
				<view class="sub-total">共1件</view>
				<view class="sub-price">
					<span>合计: </span>
					<span>{{sum}}U积分</span>
				</view>
			</view>
			<view class="sub-r">
				<button @click="submit">提交订单</button>
			</view>
		</view>
		<!-- 弹出层 -->
		<u-popup v-model="showPopup" mode='bottom' border-radius="24">
			<view class="popup-content">
				<u-icon name="close" @click='showPopup=false'></u-icon>
				<view class="popup-type">待支付</view>
				<view class="popup-price">{{sum}}U积分</view>
				<view class="popup-num">
					<span>币秋缠论指标系统/月</span>
					<span>×{{num}}</span>
				</view>
				<view class="popup-pay-type">
					<span>余额<span>(余额不足)</span></span>
					<span>1600U积分</span>
				</view>
				<u-button :disabled="true" @click="zhifu">支付</u-button>
			</view>
		</u-popup>
		<!-- 弹出层(支付) -->
		<u-popup v-model="showPopup2" mode="center" border-radius="20">
			<view class="pay-box">
				<u-icon name="close" @click='showPopup2=false'></u-icon>
				<view class="pay-title">请输入支付密码</view>
				<view class="pay-price">待支付:2000U积分</view>
				<view class="pay-pwd">
					<u-message-input :maxlength='6' dot-fill width='60'></u-message-input>
				</view>

			</view>
		</u-popup>
		<!-- 模态框-->
		<u-modal v-model="showModel" :content="content" :confirm-text='confirmText' show-cancel-button
			:show-title='false' :cancel-style='{borderRight:"1rpx solid #eee"}'></u-modal>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				num: 1,
				form: {
					name: '',
					phone: '',
					wechat: '',
				},
				showPopup: false,
				showModel: false, //模态框的显示隐藏
				content: '当前账户余额不足', //模态框的内容
				confirmText: '去充值', //模态框确定按钮文字
				showPopup2: false, //支付密码弹框
				month: "",
				total: "",
				sum:'',
				rid:''
			}
		},
		onLoad(option) {
			this.rid=option.rid;
			this.total = option.total;
			this.month = option.month;
			this.sum=parseInt(this.total*this.num);
		},
		watch:{
			num:function(val){
				console.log(val)
				this.sum=parseInt(this.total*val);
			}
		},
		
		methods: {
			submit() {
				// console.log(this.num);
				// this.$u.api.addOrder({
				// 	rid:this.rid,
				// 	orderType: 1,
				// 	name: this.form.name,
				// 	phone: this.form.phone,
				// 	wechat: this.form.wechat,
				// 	quantity: this.num
				// }).then(res => {
				// 	console.log(res)
				// })
				// this.showPopup = true;
			},
			
			about() {
				uni.reLaunch({
					url: '/pages/buy/index'
				})
			},
			zhifu(){
				console.log(111)
			}
		}
	}
</script>

<style lang="scss" scoped>
	.content {
		padding: 0 30rpx;

		// 购买的东西
		.card {
			margin-top: 30rpx;
			padding: 30rpx;
			height: 260rpx;
			border-radius: 20rpx;
			background: #fff;
			display: flex;
			justify-content: flex-start;

			img {
				width: 200rpx;
				height: 200rpx;
			}

			.card-detail {
				margin-left: 30rpx;
				width: 100%;
				font-size: 32rpx;

				.card-title {
					color: #333;
				}

				.card-price {
					margin-top: 20rpx;
					color: #FD5009;
				}

				.number-box {
					margin-top: 40rpx;
					float: right;
					display: flex;
					justify-content: space-between;
					align-items: center;

					view {
						min-width: 48rpx;
						height: 48rpx;
						text-align: center;
						border-radius: 50%;
						color: #fff;
					}

					.minus {
						background: #EEE;
					}

					.num {
						padding: 0 10rpx;
						color: #333;
					}

					.add {
						background: #FF8000;
					}
				}
			}
		}

		// 个人信息
		.user-info {
			margin-top: 30rpx;

			.info-title {
				font-size: 32rpx;
				color: #999;
			}

			.info-form {
				margin-top: 30rpx;
				padding: 30rpx;
				background: #fff;
				border-radius: 20rpx;
			}
		}

		// 服务说明
		.service {
			margin-top: 40rpx;

			view:first-of-type {
				font-size: 32rpx;
				color: #999;
			}

			view:last-of-type {
				margin-top: 30rpx;
				line-height: 48rpx;
				font-size: 28rpx;
				color: #FD5009;
			}
		}
	}

	.sub {
		position: fixed;
		bottom: 0;
		left: 0;
		padding: 17rpx 30rpx;
		width: 100%;
		height: 130rpx;
		background: #fff;
		box-shadow: 0px -3px 16px 1px rgba(0, 0, 0, 0.03);
		display: flex;
		justify-content: space-between;

		.sub-l {
			.sub-total {
				font-size: 28rpx;
				color: #999;
			}

			.sub-price {
				font-weight: bold;

				span:first-of-type {
					font-size: 28rpx;
					color: #333;
				}

				span:last-of-type {
					font-size: 40rpx;
					color: #FD5009;
				}
			}
		}

		.sub-r {
			button {
				width: 240rpx;
				height: 96rpx;
				line-height: 96rpx;
				font-size: 32rpx;
				color: #fff;
				border-radius: 48rpx;
				background: linear-gradient(128deg, #FD5009 0%, #FF8000 100%);
			}
		}
	}

	// 弹出层
	.popup-content {
		padding: 30rpx;

		.popup-type {
			margin-top: 40rpx;
			text-align: center;
			font-size: 32rpx;
			font-weight: bold;
			color: #333;
		}

		.popup-price {
			margin-top: 20rpx;
			text-align: center;
			font-size: 48rpx;
			font-weight: bold;
			color: #FD5009;
		}

		.popup-num {
			margin-top: 80rpx;
			display: flex;
			justify-content: space-between;
			font-size: 32rpx;
			color: #333;

			span:last-of-type {
				color: #999;
			}
		}

		.popup-pay-type {
			margin-top: 40rpx;
			font-size: 32rpx;
			color: #333;
			display: flex;
			justify-content: space-between;

			span:first-of-type {
				span {
					font-size: 28rpx;
					color: #FD5009;
				}
			}

			span:last-of-type {
				color: #999;
			}
		}

		button {
			margin-top: 60rpx;
			height: 96rpx;
			line-height: 96rpx;
			font-size: 32rpx;
			color: #fff;
			border-radius: 48rpx;
			background: linear-gradient(122deg, #F9662A 0%, #FF7F00 100%);
		}
	}

	// 支付
	.pay-box {
		padding: 30rpx;
		width: 590rpx;

		.pay-title {
			margin-top: 40rpx;
			text-align: center;
			font-size: 36rpx;
			font-weight: bold;
			color: #333;
		}

		.pay-price {
			margin-top: 30rpx;
			text-align: center;
			font-size: 28rpx;
			color: #FD5009;
		}

		.pay-pwd {
			margin-top: 30rpx;

		}
	}
</style>
