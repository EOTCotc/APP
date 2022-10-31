<template>
	<view>
		<view class="box" style="padding-bottom: 100rpx;">
			<view class="account">
				<u-image width="100rpx" height="100rpx" src="../../static/images/my/logo.png"></u-image>
				<view class="user">
					<text style="font-weight: 600;">{{user.mail}}</text>
					<text>UID:{{user.uid}}</text>
				</view>
			</view>
			<view class="integral">
				<view class="one">
					<text>E积分</text>
					<text class="num">{{EOTC}}</text>
				</view>
				<view class="one two">
					<text>U积分</text>
					<text class="num">{{USDT}}</text>
				</view>
			</view>
			<view>
				<u-cell-group :border="false">
					<u-cell-item title="DID认证" :arrow="true" :border-bottom="false" @click="authority()">
						<u-image slot="icon" width="40rpx" height="40rpx" style="margin-right:  10rpx"
							src="../../static/images/my/did.png"></u-image>
					</u-cell-item>
					<u-cell-item title="我的订单" :arrow="true" :border-bottom="false" @click="order()">
						<u-image slot="icon" width="40rpx" height="40rpx" style="margin-right:  10rpx"
							src="../../static/images/my/order.png"></u-image>
					</u-cell-item>
					<u-cell-item title="抽奖记录" :arrow="true" :border-bottom="false" @click="refflere()">
						<u-image slot="icon" width="40rpx" height="40rpx" style="margin-right:  10rpx"
							src="../../static/images/my/chou.png"></u-image>
					</u-cell-item>
					<u-cell-item title="交易密码" :arrow="true" :border-bottom="false" @click="transPsd()">
						<u-image slot="icon" width="40rpx" height="40rpx" style="margin-right:  10rpx"
							src="../../static/images/my/jiao.png"></u-image>
					</u-cell-item>
					<u-cell-item title="联系我们" :arrow="true" :border-bottom="false" @click="contact()">
						<u-image slot="icon" width="40rpx" height="40rpx" style="margin-right:  10rpx"
							src="../../static/images/my/lian.png"></u-image>
					</u-cell-item>
					<u-cell-item title="退出登录" :arrow="true" :border-bottom="false" @click="lot_out()">
						<u-image slot="icon" width="40rpx" height="40rpx" style="margin-right:  10rpx"
							src="../../static/images/my/tui.png"></u-image>
					</u-cell-item>
				</u-cell-group>
			</view>
		</view>
		<!-- did认证弹框 -->
		<u-modal title="DID认证" v-model="show" :content="content" :show-cancel-button="true" confirm-text="确定"
			@confirm="sure()"></u-modal>
		<!-- 退出登录 -->
		<u-modal title="退出提示" v-model="show_out" :content="out" :show-cancel-button="true" confirm-text="确定"
			@confirm="confirm()" :content-style="conStyle" confirm-color="#666"></u-modal>
		<!-- 底部组件 -->
		<biqiu-tarbar :active="active"></biqiu-tarbar>
	</view>
</template>

<script>
	import biqiuTarbar from "@/components/biqiu-tarbar.vue";
	export default {
		components: {
			biqiuTarbar,
		},
		data() {
			return {
				active: 3,
				list: ['关于恢复交易所释放空投的公告'],
				show: false,
				show_out: false,
				content: "即将前往DID去中心化认证系统",
				out: "确定要退出当前账号吗？",
				conStyle: {
					color: "#FD5009"
				},
				user: {},
				EOTC: null,
				USDT: null,
			}
		},
		created() {
			this.$u.api.getuserinfo().then(res => {
				this.user = res.items;
				this.query()
				uni.setStorageSync("user", this.user)
			});
			// let data={uid:this.uid,pwd:this.pwd};
			// this.$u.api.getQuery(data).then(res=>{
			// 	console.log(res)
			// })
		},
		methods: {
			query() {
			    let pwd=uni.getStorageSync('pwd')
			    uni.request({
			     // url:'https://api.eotcyu.club/api/OTC/QueryPoints?uid=325197&pwd=4b9a4d9e1cffcc662c8401a7f0a757a7',
			      url: 'https://api.eotcyu.club/api/OTC/QueryPoints?uid=' + this.user.uid + '&pwd=' + pwd,
			     method: 'post',
			     success: (res) => {
			      this.USDT = res.data.USDT;
			      this.EOTC = res.data.EOTC;
			     }
			    })
			   },
			about() {},
			// did认证
			authority() {
				this.show = true;
			},
			//did认证确定
			sure() {
				// #ifdef H5
				window.location.href = 'https://did.eotc.im'
				// #endif
				// #ifdef APP-PLUS
				plus.runtime.openURL('https://did.eotc.im')
				// #endif
				console.log(222)
			},
			// 退出登录	
			lot_out() {
				this.show_out = true;
			},
			//退出确定
			confirm() {
				try {
					uni.removeStorageSync('userToken');
					uni.removeStorageSync('user');
					uni.removeStorageSync('pwd');
					uni.redirectTo({
						url: '/pages/login/index'
					})
				} catch (e) {
					// error
				}
			},
			//抽奖记录
			refflere() {
				uni.redirectTo({
					url: '/pages/user/rafflerecord/rafflerecord'
				})
			},
			//交易密码
			transPsd() {
				uni.redirectTo({
					url: '/pages/user/transactionPsd/transactionPsd'
				})
			},
			//联系我们
			contact() {
				uni.redirectTo({
					url: '/pages/user/contact/index'
				})
			},
			//我的订单
			order() {
				uni.redirectTo({
					url: '/pages/user/order/order'
				})
			}
		}
	}
</script>

<style lang="scss" scoped>
	page {
		background: linear-gradient(to bottom left, #f2f7ff, 10%, #FFFFFF);
	}

	.account {
		background: none;
		margin-top: 130rpx;
		margin-left: 35rpx;
		float: left;
		display: flex;
		align-items: center;

		.user {
			margin-left: 10rpx;
			display: flex;
			font-size: 33rpx;
			flex-direction: column;
			line-height: 60rpx;
		}
	}

	.integral {
		clear: both;
		display: flex;
		justify-content: space-around;
		margin-bottom: 40rpx;
		
		.one {
			width: 43%;
			height: 145rpx;
			margin-top: 35rpx;
			background-color: #F6FAFF;
			color: #237FF8;
			border-radius: 10rpx;
			display: flex;
			flex-direction: column;
			justify-content: center;
			line-height: 55rpx;
			font-size: 30rpx;
			padding-left: 30rpx;
		}

		.num {
			font-size: 45rpx;
			font-weight: bold;
		}

		.two {
			background-color: #EFFFF0;
			color: #4DB978;
		}
	}

	.notice {
		margin: 30rpx;
	}

	/deep/.u-cell-item-box[data-v-5723aa40] {
		background: none;
	}

	/deep/ .u-line-1 {
		font-weight: bold;
	}
</style>
