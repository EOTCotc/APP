<template>
	<view class="curriculum_wrap">
		<u-navbar :is-back="true" :border-bottom="false" back-icon-color="#FFF"
			:background="{background: 'transparent'}"></u-navbar>
		<u-toast ref="toast" />
		<!-- <u-navbar :is-fixed="true" :is-back="true" :border-bottom="false" back-icon-color="#FFF" :background="{background: 'transparent'}"></u-navbar> -->
		<view class="banner">
			<u-swiper height="500" :list="detail.images"></u-swiper>
		</view>
		<view class="wrap">
			<view class="good_title u-line-1">
				<good-level :level="detail.grade" />
				<text class="text">{{detail.name}}</text>
			</view>
			<view class="buy">
				<text class="count">{{detail.buyNum}}人购买</text>
				<text class="price">{{detail.price}}U</text>
			</view>
		</view>
		<u-gap height="16" bg-color="#F5F6F7"></u-gap>
		<u-tabs :list="list" :is-scroll="false" :current="current" inactive-color="#999" active-color="#333" @change="change"></u-tabs>
		<view class="teach_wrap content" v-show="current === 0" v-if="!!detail.teachers">
			<view class="flex" style="justify-content: flex-start;">
				<u-avatar :src="transformSrc(detail.teachers[0].headImage)"></u-avatar>
				<view class="block">
					<view class="name">{{detail.teachers[0].name}}</view>
				</view>
			</view>
			<view class="info">{{detail.teachers[0].blurb}}</view>
		</view>
		<view class="detail_wrap content" v-show="current === 1">
			<view class="item">
				<view class="header">
					<u-icon class="icon" name="play-right-fill"></u-icon>
					<text class="title">{{detail.blurbTitle || '课程简介'}}</text>
				</view>
				<view class="text">{{detail.blurb}}</view>
			</view>
			<view class="item">
				<view class="header">
					<u-icon class="icon" name="play-right-fill"></u-icon>
					<text class="title">{{detail.contentTitle || '主讲内容'}}</text>
				</view>
				<view class="text">{{detail.content}}</view>
			</view>
		</view>
		<view class="btn">
			<button
				@click="buy"
			>
				购买
			</button>
		</view>
	</view>
</template>

<script>
	import goodLevel from "@/components/good-level.vue"
	import { transformSrc } from "@/common/utils/transform.js"
	
	export default {
		components: {
			goodLevel
		},
		data() {
			return {
				current: 0,
				detail: {},
				list: [{
					name: '讲师介绍'
				}, {
					name: '课程详情'
				}],
			}
		},
		created() {
		},
		onLoad(options) {
			console.log(options.id,'options')
			this.$u.api.getCourseDetail(options.id).then(res => {
				const data = res.items
				data.images = data.images.split(',').map(item => this.transformSrc(item))
				this.detail = data
				console.log('data', data)
			})
		},
		methods: {
			transformSrc,
			change(data) {
				this.current = data
			},
			buy() {
				this.$refs.toast.show({
					title: '暂未开放！',
					type: "wraning"
				})
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
		.content {
			padding: 30rpx 30rpx 200rpx;
			border-top: 1rpx solid #F5F6F7;
		}
		.teach_wrap {
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
			left: 0;
			right: 0;
			bottom: 0;
			padding: 30rpx;
			box-sizing: border-box;
			background-color: #FFF;
			border-top: 1px solid #EEE;
			button {
				color: #FFF;
				border: none;
				outline-style: none;
				border-radius: 40px;
				font-size: 32rpx;
				background: linear-gradient(128deg, #FD5009 0%, #FF8000 100%);
			}
		}
	}
</style>
