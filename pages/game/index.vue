<template>
	<view class="warp">
		<!-- <web-view :src="src" fullscreen :webview-styles="webviewStyles"></web-view> -->
		<u-navbar :is-back="false" :title="homeTitle" :title-color="titleColor"></u-navbar>
		<view class="box1">
			<view class="bg-purple list">
				<u-image src="../../static/images/game/img1.png" width="175rpx" height="130rpx" border-radius="25rpx"></u-image>
				<view class="text">幸运大转盘</view>
				<button size="mini" style="background-color: #C4C4C4  ;" disabled @click="spinwin">暂未开始</button>
			</view>
			<view class="bg-purple-light list">
				<u-image src="../../static/images/game/img2.png" width="175rpx" height="130rpx" border-radius="25rpx">
				</u-image>
				<view class="text">砸金蛋</view>
				<button size="mini" style="background-color: #C4C4C4  ;" disabled @click="smashgoldeneggs">暂未开始</button>
			</view>

			<view class="bg-purple-dark list">
				<u-image src="../../static/images/game/img4.png" width="175rpx" height="130rpx" border-radius="25rpx">
				</u-image>
				<view class="text">幸运大翻盘</view>
				<button size="mini" style="background-color: #C4C4C4  ;" disabled @click=" bigreversal ">暂未开始</button>
			</view>

		</view>
		<view class="box2">
			<view class=" bg-purple-red list">
				<u-image src="../../static/images/game/img5.png" width="175rpx" height="130rpx" border-radius="25rpx">
				</u-image>
				<view class="text">刮刮乐</view>
				<button size="mini" style="background-color: #C4C4C4 ;" disabled>暂未开始</button>
			</view>
			<view class="bg-purple-yellow list">
				<u-image src="../../static/images/game/img3.png" width="175rpx" height="130rpx" border-radius="25rpx">
				</u-image>
				<view class="text">全民抓福袋</view>
				<button size="mini" style="background-color: #C4C4C4  ;" disabled>暂未开始</button>
			</view>
			<view class="list"></view>
		</view>
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
				src: null,
				active: 2,
				homeTitle: "游戏大厅",
				background: "#fff",
				titleColor: "#333333",
				webviewStyles: {
					width: '100%',
					height: '100%'
				}
			};
		},
		methods: {
			spinwin() {
				// this.src = "https://uniapp.dcloud.io/static/web-view.html"
				// console.log(this.src, 'this.src')
				uni.reLaunch({
					url: '/pages/game/spinwin/index'
				})
			},
			smashgoldeneggs() {
				uni.reLaunch({
					url: '/pages/game/smashgoldeneggs/smashgoldeneggs'
				})
			},
			bigreversal() {
				uni.reLaunch({
					url: '/pages/game/bigreversal/bigreversal'
				})
			}
		}
	}
</script>

<style lang="scss" scoped>
	.wrap {
		padding: 34rpx;
	}

	uni-button:after {
		border: none !important;

	}

	uni-button {
		color: #fff;
		width: 170rpx;
		border-radius: 20rpx;
	}

	uni-button[disabled]:not([type]) {
		color: #fff;
	}

	::v-deep .u-navbar-inner[data-v-1194bf80] {
		background-color: #fff;
	}

	.box1 {
		display: flex;
		justify-content: space-evenly;
		align-items: center;
		margin: 30rpx 0;
	}

	.box2 {
		display: flex;
		align-items: center;
		justify-content: space-evenly;

	}

	.list {
		display: flex;
		flex-direction: column;
		justify-content: space-evenly;
		align-items: center;
		height: 270rpx;
		width: 210rpx;

		.text {
			color: #fff;
		}

		.u-image {}
	}

	.demo-layout[data-v-f64c19a4] {
		height: 270rpx;
	}

	.u-row {
		margin: 40rpx 0 0 0;
	}

	.demo-layout {
		height: 80rpx;
		border-radius: 8rpx;
	}

	.bg-purple {
		background: linear-gradient(to top, #FFA42D, #FF7827);
	}

	.bg-purple-light {
		background: linear-gradient(to top, #FF8585, #F95C3B);
	}

	.bg-purple-dark {
		background: linear-gradient(to top, #DF92FF, #9B45E1);
	}

	.bg-purple-red {
		background: linear-gradient(to top, #F38C4D, #E54728);
	}

	.bg-purple-yellow {
		background: linear-gradient(to top, #FFCC67, #FCAA2E);
	}
</style>
