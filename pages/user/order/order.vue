<template>
	<view>
		<!--头部组件-->
		<u-navbar :is-back="true" :title="homeTitle" :custom-back="about"></u-navbar>

		<view>
				<u-image width="50%" height="240rpx" style="margin:30% auto 0;" mode="widthFix" src="../../../static/images/my/notdata.png"></u-image>
				<view style="text-align:center;color:#999">暂无订单信息</view>
			<!-- <u-tabs :list="tab.data" :is-scroll="false" :current="tab.active" @change="change"></u-tabs>
			<view v-for="item in orderArr">
				<u-card :title="item.orderId" @click="detail(item.orderId)" sub-title-size="30rpx"
					:sub-title="item.status==0?'待支付':item.status==1?'已支付':'已取消'" :foot-border-top="false"
					:sub-title-color="item.status==0?'#FD5009':item.status==1?'#2AAC2E':'#999999'"
					style="border-radius:32rpx;">
					<view slot="body">
						<view class="col">
							<view class="col_one">
								<u-image width="160rpx" height="160rpx" src="../../../static/images/my/chu.png">
								</u-image>
								<view class="wen" v-if="item.orderType==0">
									<text>{{item.course.name}}</text>
									<text>{{item.course.price}}U积分/月</text>
									<text class="hui">{{item.createDate|timeFormat('yyyy/mm/dd hh:ss')}}</text>
								</view>
								<view class="wen" v-if="item.orderType==1">
									<text>{{item.clSystem.name}}</text>
									<text>{{item.clSystem.price}}U积分/月</text>
									<text>{{item.createDate|timeFormat('yyyy/mm/dd hh:ss')}}</text>
								</view>
							</view>
							<view class="num" v-if="item.quantity!=0">x{{item.quantity}}</view>
						</view>
						<view class="total" v-if="item.orderType==0">总价:<text style="color:#FD5009">{{item.course.price*item.quantity}}U积分</text>
						</view>
						<view class="total" v-if="item.orderType==1">总价:<text style="color:#FD5009">{{item.clSystem.price*item.quantity}}U积分</text></view>
						<view class="btn" v-if="item.status==0">
							<u-button shape="circle" type="primary" class="btn_qu" hover-class="none"
								@click="cencle(item.orderId)">取消</u-button>
							<u-button shape="circle" class="btn_zhi" hover-class="{color:#fff}"
								@click="payment(item.orderId)">支付
							</u-button>
						</view>
					</view>
				</u-card>
			</view>-->
		</view> 
	</view>
</template>

<script>
	export default {
		data() {
			return {
				homeTitle: "我的订单",
				tab: {
					data: [{
						name: '全部',
					}, {
						name: '待支付',
					}, {
						name: '已支付',
					}],
					active: 0,
				},
				status: undefined,
				// orderArr: [
				// 	{orderId:'11111',status:0,orderType:0,createDate:'2022/02/23'},{orderId:'22222',status:1,orderType:0,createDate:'2022/02/23'},{orderId:'22222',status:2,orderType:0,createDate:'2022/02/23'}
				// ],
				orderArr: []
			}
		},
		created() {
			this.getorder();
			// 			this.$u.api.apporder({
			//   rid: "string",
			//   orderType: 0,
			//   name: "string",
			//   phone: "string",
			//   wechat: "string",
			//   quantity: 0
			// }).then(res=>{

			// 			})
		},
		methods: {
			//获取订单
			getorder() {
				this.$u.api.getOrderbyuser({
					status: this.status
				}).then(res => {
					this.orderArr = res.items;
					// console.log(this.orderArr)
				})
			},
			//tabs切换
			change(index) {
				this.tab.active = index;
				if (this.tab.active == 0) {
					this.status = undefined;
				} else if (this.tab.active == 1) {
					this.status = 0;
				} else if (this.tab.active == 2) {
					this.status = 1
				}
				this.getorder()
				console.log(index, this.status)
			},
			//支付
			payment(orderid) {
				console.log(orderid)
				let data = {
					orderid: orderid
				}
				this.$u.api.payorder(data).then(res => {
					console.log(res)
					if (res.code == 0) {
						this.getorder();
					}
				});
			},
			//取消
			cencle(orderid) {
				this.$u.api.cancelorder({
					orderid: orderid
				}).then(res => {
					if (res.code == 0) {
						this.getorder();
					}
				});

			},
			detail(orderid) {
				console.log(orderid)
				uni.navigateTo({
					url: `/pages/user/order/order_detail?orderid=${orderid}`
				})
			},
			//返回
			about() {
				uni.reLaunch({
					url: '/pages/user/index'
				})
			}
		}
	}
</script>

<style lang="scss" scoped>
	.col {
		display: flex;
		justify-content: space-between;
		align-items: center;

		.col_one {
			display: flex;
			justify-content: center;
			align-items: center;

			.wen {
				display: flex;
				flex-direction: column;
				line-height: 60rpx;
				margin-left: 20rpx;

				.hui {
					color: #999;
				}
			}
		}
	}

	.total {
		margin-top: 20rpx;
		font-size: 30rpx;
		font-weight: 550;
	}

	.btn {
		display: flex;
		float: right;
		margin: 30rpx 10rpx 30rpx 0;

		/deep/.u-size-default[data-v-6e15e680] {
			width: 160rpx;
			height: 64rpx;
		}

		.btn_qu {
			background: #F5F6F7;
			color: #999;
			margin-right: 20rpx;
		}

		.btn_zhi {
			background: linear-gradient(to right, #FD5009, #FF8000);
			color: #fff;
		}
	}

	.num {
		font-size: 32rpx;
	}

	/deep/.u-card__head__title__text {
		font-weight: bold;
	}
</style>
