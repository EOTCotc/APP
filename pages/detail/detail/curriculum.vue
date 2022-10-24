<template>
	<view class="curriculum_wrap">
		<u-navbar :is-back="true" :border-bottom="false" back-icon-color="#FFF"
			:background="{background: 'transparent'}"></u-navbar>
		<!-- <u-navbar :is-fixed="true" :is-back="true" :border-bottom="false" back-icon-color="#FFF" :background="{background: 'transparent'}"></u-navbar> -->
		<view class="banner">
			<image class="banner-image-item" src="/static/images/common/img2.png" mode="widthFix"></image>
		</view>
		<view class="wrap">
			<view class="good_title u-line-1">
				<good-level :level="1" />
				<text class="text">缠论初级课程</text>
			</view>
			<view class="buy">
				<text class="count">40人购买</text>
				<text class="price">1000U</text>
			</view>
		</view>
		<u-gap height="16" bg-color="#F5F6F7"></u-gap>
		<u-tabs :list="list" :is-scroll="false" :current="current" inactive-color="#999" @change="change"></u-tabs>
		<view class="teach_wrap" v-show="current === 0">
			<view class="flex">
				<view class="flex">
					<u-avatar src="http://pic2.sc.chinaz.com/Files/pic/pic9/202002/hpic2119_s.jpg"></u-avatar>
					<view class="block">
						<view class="name">许沐沐老师</view>
						<view class="follow">关注人数:188</view>
					</view>
				</view>
				<view>
					<u-button plain size="mini" shape="circle" type="primary">主要按钮</u-button>
				</view>
			</view>
			<view class="info">
				许沐沐老师是上海开缠门电子商务有限公司创始人，自2008年便开始研究缠论，并利用缠论理论建立量化投资模型，奠定了软件自动化缠论交易的基础。可以毫不夸张的说，许沐沐老师对于缠论的实际运用和理论研究，已经到了一个极为高深的境界，并且，她正在用着自己的方式，影响着这个行业，改变着这个行业。
			</view>
		</view>
		<view class="detail_wrap" v-show="current === 1">
			<view class="item">
				<view class="header">
					<u-icon class="icon" name="play-right-fill"></u-icon>
					<text class="title">课程简介</text>
				</view>
				<view class="text">
					“缠论是网络名人缠中说禅关于证券市场的一套投资理论，因其发表于缠师108篇博文内，故理论的逻辑层次很难为初学者所熟知。本书编者在经过大量的理论研究与实战验证之后，对缠论博文进行了系统性的梳理，重建了逻辑层次与理论框架，从买卖策略和心态调整方面指导股民了解股市及缠论。缠论是目前独一能用数学手段进行证明的交易理论。”
				</view>
			</view>
			<view class="item">
				<view class="header">
					<u-icon class="icon" name="play-right-fill"></u-icon>
					<text class="title">主讲内容</text>
				</view>
				<view class="text">
					“缠论是网络名人缠中说禅关于证券市场的一套投资理论，因其发表于缠师108篇博文内，故理论的逻辑层次很难为初学者所熟知。本书编者在经过大量的理论研究与实战验证之后，对缠论博文进行了系统性的梳理，重建了逻辑层次与理论框架，从买卖策略和心态调整方面指导股民了解股市及缠论。缠论是目前独一能用数学手段进行证明的交易理论。”
				</view>
			</view>
		</view>
		<view class="btn">
			<u-button shape="circle" type="primary"
				style="background: linear-gradient(128deg, #FD5009 0%, #FF8000 100%);">购买</u-button>
		</view>
	</view>
</template>

<script>
	import goodLevel from "@/components/good-level.vue"
	export default {
		components: {
			goodLevel
		},
		data() {
			return {
				current: 0,
				list: [{
					name: '讲师介绍'
				}, {
					name: '课程详情'
				}],
			}
		},
		methods: {
			change(data) {
				this.current = data
			}
		}
	}
</script>

<style scoped lang="scss">
	.curriculum_wrap {
		min-height: 100vh;
		background-color: #FFF;

		.banner {
			margin-top: -88rpx;

			.banner-image-item {
				width: 100%;
			}
		}

		.wrap {
			padding: 30rpx;
			margin-bottom: 20rpx;

			.good_title {
				display: flex;
				align-items: center;
				font-size: 36rpx;
				color: #333;

				.text {
					margin-left: 20rpx;
				}
			}

			.buy {
				display: flex;
				justify-content: space-between;
				align-items: center;
				margin-top: 30rpx;

				.count {
					font-size: 28rpx;
					color: #999;
				}

				.price {
					font-size: 40rpx;
					color: #FD5009;
				}
			}
		}

		.teach_wrap {
			margin-top: 1rpx;
			padding: 30rpx 30rpx 120rpx;
			border-top: 1rpx solid #F5F6F7;

			.flex {
				display: flex;
				justify-content: space-between;
				align-items: center;
			}

			.block {
				margin-left: 20rpx;
			}

			.name {
				color: #333333;
				font-weight: bold;
				font-size: 32rpx;
			}

			.follow {
				color: #333333;
				font-size: 28rpx;
				margin-top: 6rpx;
			}

			.info {
				color: #333;
				font-size: 28rpx;
				line-height: 56rpx;
				margin-top: 40rpx;
			}
		}

		.detail_wrap {
			margin-top: 1rpx;
			padding: 30rpx 30rpx 120rpx;
			border-top: 1rpx solid #F5F6F7;

			.item {
				margin-bottom: 30rpx;

				.header {
					.icon {
						color: #A7C1FF;
						margin-right: 10rpx;
					}

					.title {
						color: #333;
						font-size: 32rpx;
						font-weight: bold;
					}
				}

				.text {
					color: #333;
					font-size: 28rpx;
					line-height: 56rpx;
					padding: 30rpx 0;
					border-bottom: 1rpx solid #EEE;
				}
			}
		}

		.btn {
			position: fixed;
			left: 30rpx;
			right: 30rpx;
			bottom: 30rpx;
		}
	}
</style>
