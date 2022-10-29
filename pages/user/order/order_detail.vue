<template>
	<view>
		<!--头部组件-->
		<u-navbar :is-back="true" :title="homeTitle" :custom-back="about"></u-navbar>
		<view class="box">
			<view class="wen">
				<text>{{list.name}}</text>
				<text style="margin-left:20rpx">{{list.phone}}</text>
			</view>
			<u-button shape="circle" class="btn_qu" @click="update()" v-if="list.status==0">修改</u-button>
			<view class="zhuang se" v-if="list.status==1">
				<u-icon name="checkmark-circle" color="#2AAC2E" size="35" style="margin-right:10rpx;"></u-icon>已支付
			</view>
			<view class="zhuang" v-if="list.status==2">
				<u-icon name="close-circle" color="#999999" size="35" style="margin-right:10rpx;"></u-icon>已取消
			</view>
		</view>
		<view class="xiang" style="padding-bottom: 300rpx;">
			<view class="shang">
				<u-image width="200rpx" height="200rpx" src="../../../static/images/my/chu.png"></u-image>
				<view class="deta_wen">
					<text>{{list.clSystem.name}}</text>
					<text>{{list.clSystem.price}}U积分</text>
				</view>
			</view>
			<view class="ji_fen">
				<view>总价</view>
				<view style="color:#FD5009;font-weight: 600;">{{list.clSystem.price*list.quantity}}U积分</view>
			</view>
			<view class="ji_fen">
				<view>订单编号</view>
				<view class="fu">{{list.orderId}}
					<u-image width="26rpx" style="margin-left:20rpx" height="26rpx" @click="copy(list.orderId)"
						src="../../../static/images/my/fu.png"></u-image>
				</view>
			</view>
			<view class="ji_fen">
				<view>下单时间</view>
				<view class="fu">{{list.createDate|timeFormat('yyyy-mm-dd hh:ss')}}</view>
			</view>
			<view class="ji_fen" v-if="list.status==1">
				<view>付款时间</view>
				<view class="fu">{{list.paymentDate|timeFormat('yyyy-mm-dd hh:ss')}}</view>
			</view>
			<view class="ji_fen" v-if="list.status==2">
				<view>取消时间</view>
				<view class="fu">{{list.cancelDate|timeFormat('yyyy-mm-dd hh:ss')}}</view>
			</view>
			<u-line color="#F5F6F7" />
			<view class="volunt" v-if="list.status==1">
				<view class="name">志愿者微信</view>
				<view class="title_name">
					<view class="tu_name">
						<u-image style="margin-right:20rpx;" width="40rpx" height="34rpx"
							src="../../../static/images/my/wei.png"></u-image>
						<view>{{list.wechat}}</view>
					</view>
					<view>
						<u-button type="primary" shape="circle" class="btn" @click="copy(list.wechat)">复制</u-button>
					</view>
				</view>
				<view class="er">
					<u-image width="330rpx" height="330rpx" src="../../../static/images/my/ewm.png"></u-image>
					<view>扫一扫添加</view>
				</view>
				<view class="conter">支付成功后48小时内会有志愿者为您开通系统账号并联系您，可提前添加志愿者微信，等待志愿者为您开通。</view>
			</view>
		</view>
		<!-- 修改弹框 -->
		<u-modal v-model="show" title="修改个人信息" :show-confirm-button="false" width="90%" :mask-close-able="true">
			<u-form :model="form" ref="uForm" style="padding:20rpx 40rpx;" validate-trigger="bind">

				<u-form-item label="姓名" prop="name" label-width="160rpx" :required="true">
					<u-input v-model="form.name" placeholder="请输入真实姓名" />
				</u-form-item>
				<u-form-item label="手机号" prop="phone" label-width="160rpx" :required="true">
					<u-input v-model="form.phone" placeholder="请填写您的手机号码" />
				</u-form-item>
				<u-form-item label="微信号" prop="wechat" label-width="160rpx" :required="true">
					<u-input v-model="form.wechat" placeholder="请填写您的微信号" />
				</u-form-item>
				<view class="slot-title" style="position: absolute; right:40rpx;top:54rpx;">
					<u-image width="24rpx" height="24rpx" src="../../../static/images/my/cha.png"
						@click="submit(list.orderId)">
					</u-image>
				</view>
			</u-form>
			<view style="color:#FD5009;padding:30rpx 20rpx;">支付成功后48小时内会有志愿者为您开通系统账号并联系您，请填写正确的个人信息。</view>
		</u-modal>
		<!-- 底部 -->
		<view class="foot" v-if="list.status==0">
			<u-button shape="circle" class="btn_qu" type="primary" hover-class="none" @click="cencle(list.orderId)">取消
			</u-button>
			<u-button shape="circle" class="btn_zhi" @click="payment(list.orderId)">支付</u-button>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				homeTitle: '订单详情',
				show: false,
				id: '',
				form: {
					name: undefined, //姓名
					phone: undefined, //手机号
					wechat: undefined //微信号
				},
				rules: {
					name: [{
						required: true,
						message: '请输入真实姓名',
						trigger: ['blur', 'change']
					}],
					phone: [{
						required: true,
						message: '请填写您的手机号',
						trigger: ['blur', 'change']
					}, {
						validator: (rule, value, callback) => {
							return this.$u.test.mobile(value);
						},
						message: '手机号码不正确',
						trigger: ['change', 'blur'],
					}, ],
					wechat: [{
						required: true,
						message: '请填写您的微信号',
						trigger: ['blur', 'change']
					}],
				},
				// list: {name:"莉莉",phone:"1398549303",orderId:'11111',wechat:'were33444',status:2,paymentDate:'2022/02/23',cancelDate:'2022-02-23',createDate:'2022/02/23'},
				list:{}
			};
		},

		watch: {
			show(newVal) {
				if (newVal)
					this.$nextTick(() => {
						this.$refs.uForm.setRules(this.rules);
						// this.$refs['uForm'].resetFields();  重置
					});

			}
		},
		onLoad(option) {
			this.id = option.orderid;
		},
		created() {
			this.getOrderby()
		},
		methods: {
			getOrderby() {
				this.$u.api.getOrderby({
					id: this.id
				}).then(res => {
					console.log(res)
					this.list = res.items;
				})
			},

			about() {
				uni.reLaunch({
					url: '/pages/user/order/order'
				})
			},
			update() {
				this.show = true;
				this.form.name=this.list.name;
				this.form.phone=this.list.phone;
				this.form.wechat=this.list.wechat;
			},
			//弹框关闭按钮
			submit(orderid) {
				this.$refs.uForm.validate(valid => {
					if (valid) {
						let data = {
							name: this.form.name,
							phone: this.form.phone,
							wechat: this.form.wechat,
							orderId: orderid
						}
						console.log(data)
						this.$u.api.updateOrder(data).then(res => {
							console.log(res)
							this.getOrderby();
						})
						this.show = false;
						
					} else {
						console.log(valid, '验证失败');
					}
				});
			},
			//取消
			cencle(orderid) {
				this.$u.api.cancelorder({
					orderid: orderid
				}).then(res => {
					if (res.code == 0) {
						this.getOrderby();
					}
				})

			},
			//支付
			payment(orderid) {
				console.log(orderid)
				let data = {
					orderid: orderid
				}
				this.$u.api.payorder(data).then(res => {
					if (res.code == 0) {
						this.getOrderby();
					}
				})

			},
			// 复制功能
			copy(value) {
				uni.setClipboardData({
					data: value, //要被复制的内容
					success: () => { //复制成功的回调函数
						uni.showToast({ //提示
							title: "复制成功"
						})
					}
				});
			}
		}
	}
</script>

<style lang="scss" scoped>
	.box {
		margin: 30rpx auto;
		background-color: #fff;
		border-radius: 20rpx;
		width: 92%;
		height: 100rpx;
		display: flex;
		justify-content: space-between;
		align-items: center;

		.wen {
			color: #333;
			font-size:30rpx;
			padding-left: 30rpx;
		}

		/deep/.u-size-default[data-v-6e15e680] {
			width: 80rpx;
			height: 50rpx;
		}

		.btn_qu {
			color: #999;
			margin-right: 20rpx;
			padding:0 50rpx;
		}

		.zhuang {
			margin-right: 20rpx;
			font-size: 28rpx;
			font-weight: bold;
			color: #999;
			display: flex;
		}

		.se {
			color: #2AAC2E;
		}
	}

	.xiang {
		border-radius: 20rpx;
		background-color: #fff;
		margin: 0 auto;
		padding: 20rpx 20rpx 30rpx;
		width: 92%;
		overflow: auto;

		.shang {
			display: flex;
			margin-bottom: 30rpx;

			.deta_wen {
				font-size: 32rpx;
				display: flex;
				flex-direction: column;
				line-height: 60rpx;
				margin-left: 20rpx;
			}
		}
	}

	.ji_fen {
		display: flex;
		justify-content: space-between;
		font-size: 35rpx;
		margin-bottom:40rpx;

		.fu {
			display: flex;
			color: #999999;
		}
	}

	.foot {
		width: 100%;
		position: fixed;
		bottom: -30rpx;
		background-color: #fff;
		padding: 35rpx 0 40rpx;
		display: flex;
		margin: 30rpx 10rpx 30rpx 0;

		/deep/.u-size-default[data-v-6e15e680] {
			width: 330rpx;
			height:100rpx;
		}

		.btn_qu {
			background: #F5F6F7;
			color: #333333;
			margin-right: 20rpx;
		}

		.btn_zhi {
			background: linear-gradient(to right, #FD5009, #FF8000);
			color: #fff;
		}
	}

	.volunt {
		margin-top: 40rpx;

		.name {
			font-size: 32rpx;
			font-weight: bold;
		}

		.title_name {
			padding: 0 20rpx;
			margin-top: 35rpx;
		}

		.title_name,
		.tu_name {
			
			display: flex;
			justify-content: space-between;
			align-items: center;
			font-size:30rpx;
		}

		.btn {
			width: 120rpx;
			height: 44rpx;
		}

		.er {
			display: flex;
			justify-content: center;
			align-items: center;
			flex-direction: column;
			margin: 20rpx;
			font-size: 32rpx;
		}

		.conter {
			color: #FD5009;
			text-align: center;
		}
	}
</style>
