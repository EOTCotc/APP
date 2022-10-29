<template>
	<view>
		<u-toast ref="uToast" />
		<u-no-network></u-no-network>
		<u-navbar :border-bottom='false' :background='{background:"#F6F8F9"}'>
		</u-navbar>
		<view class="content">
			<view class="type">
				<u-icon name="checkmark-circle" size='48' color='#2AAC2E'></u-icon>
				<span>支付成功</span>
			</view>
			<view class="tip">请添加志愿者微信</view>
			<view class="wechat">
				<view class="wechat-top">
					<view class="wechat-number">
						<u-icon name="weixin-fill" size='47' color='#3CB034'></u-icon>
						<span>{{value}}</span>
					</view>
					<view class="copy" @click="copy(value)">复制</view>
				</view>
				<view class="wechat-img">
					<img src="@/static/images/buy/index_buy.png" alt="">
				</view>
				<view class="add">扫一扫添加</view>
			</view>
			<view class="explain">
				支付成功后48小时内会有志愿者为您开通系统账号并联系您，可提前添加志愿者微信，等待志愿者为您开通。
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				value:'we879635',
			
			}
		},
	
		methods: {
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
	.content {
		padding: 0 60rpx;

		.type {
			margin-top: 80rpx;
			font-size: 40rpx;
			font-weight: bold;
			color: #333;
			display: flex;
			justify-content: center;
			align-items: center;

			span {
				margin-left: 10rpx;
			}
		}

		.tip {
			margin-top: 120rpx;
			text-align: center;
			font-size: 36rpx;
			font-weight: bold;
			color: #333;
		}

		.wechat {
			margin-top: 60rpx;
			padding: 40rpx;
			border-radius: 20rpx;
			background: #fff;

			.wechat-top {
				height: 48rpx;
				display: flex;
				justify-content: space-between;
				align-items: center;

				.wechat-number {
					span {
						margin-left: 15rpx;
						font-size: 32rpx;
						color: #333;
					}
				}

				.copy {
					width: 120rpx;
					height: 48rpx;
					line-height: 48rpx;
					text-align: center;
					font-size: 28rpx;
					color: #fff;
					border-radius: 24rpx;
					background: #1758F1;
				}
			}
		}

		.wechat-img {
			margin: 60rpx auto 0;
			width: 360rpx;
			height: 360rpx;

			img {
				width: 100%;
				height: 100%;
			}
		}

		.add {
			margin-top: 40rpx;
			text-align: center;
			font-size: 32rpx;
			color: #333;
		}
		.explain{
			margin-top: 48rpx;
			line-height: 48rpx;
			font-size: 28rpx;
			color: #FD5009;
		}
	}
</style>
