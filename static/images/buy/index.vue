<template>
	<view class="container">
		<u-toast ref="uToast" />
		<u-no-network></u-no-network>
		<u-navbar title-bold back-icon-color='#fff' title='购买' title-color='#fff' :background='{background:"#0A0E29"}'></u-navbar>
		<img class='index_buy' src="@/static/images/buy/index_buy.png" alt="">
		<view class="content">
			<view class="title">全球首个区块链缠论指标系统</view>
			<view class="account-box">
				<view class="account" :class="{selected:isSelected1}" @click="handleSelected">
					<view>1个月</view>
					<view>
						<span>1000</span>
						<span>USDT</span>
					</view>
					<view>币秋账号</view>
				</view>
				<view class="account" :class="{selected:isSelected2}" @click="handleSelected">
					<view>1个月</view>
					<view>
						<span>1000</span>
						<span>USDT</span>
					</view>
					<view>币秋账号</view>
				</view>
			</view>
		</view>
		<view class="buy">
			<button>购买</button>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				isSelected1: true,
				isSelected2: false,
			}
		},
		methods: {
			handleSelected() {
				this.isSelected1 = !this.isSelected1
				this.isSelected2 = !this.isSelected2
			}
		}
	}
</script>

<style lang="scss" scoped>
	.container {
		min-height: 100vh;
		background-color: #fff;
	}

	.index_buy {
		width: 100%;
		height: 551rpx;
	}

	.content {
		padding: 0 30rpx 200rpx 30rpx;

		.title {
			margin-top: 30rpx;
			text-align: center;
			font-size: 36rpx;
			color: #333;
		}

		.account-box {
			margin-top: 80rpx;
			display: flex;
			justify-content: space-between;

			.account {
				padding: 40rpx;
				width: 330rpx;
				height: 300rpx;
				border-radius: 16rpx;
				box-shadow: 0 3rpx 16rpx 1rpx rgba(0, 0, 0, 0.05);
				display: flex;
				flex-direction: column;
				justify-content: space-between;
				align-items: center;
				color: #333;

				view:first-of-type {
					font-size: 32rpx;
				}

				view:nth-of-type(2) {
					span:first-of-type {
						font-size: 40rpx;
					}

					span:last-of-type {
						font-size: 28rpx;
					}
				}

				view:last-of-type {
					font-size: 28rpx;
					color: #999;
				}
			}

			.selected {
				border: 4rpx solid #FD5009;
				background: #FFF6F2;

				view:first-of-type {
					color: #FD5009;
				}

				view:nth-of-type(2) {
					color: #FD5009;
				}
			}
		}
	}

	.buy {
		position: fixed;
		bottom: 0;
		left: 0;
		padding: 17rpx 30rpx;
		width: 100%;
		height: 130rpx;
		box-shadow: 0 -3rpx 16rpx 1rpx rgba(0, 0, 0, 0.03);

		button {
			width: 100%;
			height: 100%;
			font-size: 32rpx;
			color: #fff;
			line-height: 96rpx;
			border-radius: 48rpx;
			background: linear-gradient(128deg, #FD5009 0%, #FF8000 100%);
		}
	}
</style>
