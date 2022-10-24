<template>
	<view class="box">
		<u-navbar :is-back="true" back-text="返回" :title-color="titleColor" :custom-back="about" :background="background"
			back-icon-color="#fff">
			<block slot="right">
				<u-button plain size="mini" :custom-style="customStyle" @click="show = true">活动规则</u-button>
			</block>
		</u-navbar>
		<!-- 活动规则弹出层 -->
		<u-popup v-model="show" mode="center" border-radius="14" closeable width="89%" height="58%"
			close-icon-color="#fff" close-icon="close-circle" close-icon-size="40">
			<view class="content">
				<view class="title">
					活动规则说明
				</view>
				<view class="text">
					1.活动时间：2022年7月15日——2023年7月15日。<br />
					2.本次活动为EOTC专属特权活动，仅针对质押用户参与。<br />
					3.活动期间，EOTC质押专属客户每天可参与抽奖。<br />
					4.本次活动奖品为 苹果13手机、苹果手表、华为笔记本电脑、NFT盲盒、50USDT、20EOTC、5EOTC。<br />
					5.苹果13手机价值5999RMB、华为笔记本电脑价值4199RMB、苹果手表价值2199RMB、NFT盲盒价值200USDT<br />
					6.此游戏中使用的EOTC将会优先使用质押收益的EOTC，若收益不足将会使用质押中的EOTC.
				</view>
			</view>
		</u-popup>
		<view class="titlebg"></view>
		<turntable></turntable>
	</view>
</template>

<script>
	// 引入转盘页面
	import turntable from '@/pages/game/spinwin/turntable/turntable.vue'
	export default {
		components:{
			turntable
		},
		data() {
			return {
				background: {
					backgroundColor: 'transparent',
					border: 'none'
				},
				// homeTitle: "幸运大转盘",
				titleColor: "#333333",
				customStyle: {
					border: '1px solid #f58d40',
					color: '#f58d40'
				},
				show: false
			};
		},
		methods: {
			about() {
				uni.reLaunch({
					url: '/pages/game/index'
				})
			}
		}
	}
</script>

<style lang="scss" scoped>
	::v-deep .u-hairline-border[data-v-6e15e680]:after {
		border: none;
	}

	::v-deep .u-border-bottom:after {
		border: none;

	}

	::v-deep uni-button {
		background-color: transparent !important;
	}

	::v-deep .u-slot-right {
		margin-right: 35rpx;
	}

	::v-deep .u-back-text[data-v-1194bf80] {
		color: #fff !important;
	}

	.box {
		height: 100%;
		width: 100%;
	}

	page {
		position: fixed;
		top: 0;
		left: 0;
		background-image: url(../../../static/images/game/image/bg.png);
		background-size: 100% 100%;
		width: 100%;
		height: 100%;
	}

	.content {
		padding: 30rpx 32rpx;
		height: 100%;
		background-color: #f58d40;
		color: #fff;

		.title {
			font-size: 36rpx;
			margin-bottom: 40rpx;
			text-align: center;
		}

		.text {
			line-height: 55rpx;
			font-size: 27rpx;
		}
	}
	.titlebg{
		background-image: url(../../../static/images/game/image/title.png);
		background-size: 100% 100%;
		width: 85%;
		height: 320rpx;
		margin: auto;
	}
</style>
