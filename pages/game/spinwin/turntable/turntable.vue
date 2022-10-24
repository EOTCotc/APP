<template>
	<view class="main">
		<view class="rotate">
			<view class="lunpai">
				<view class="prize running">
					<view class="li">
						<text></text>
						<view>5 EOTC</view>
					</view>
					<view class="li">
						<text></text>
						<view>20 EOTC</view>
					</view>
					<view class="li">
						<text></text>
						<view>苹果13</view>
					</view>
					<view class="li">
						<text></text>
						<view>50 USDT</view>
					</view>
					<view class="li">
						<text></text>
						<view>华为笔记本</view>
					</view>
					<view class="li">
						<text></text>
						<view>谢谢参与</view>
					</view>
					<view class="li">
						<text></text>
						<view>NFT盲盒</view>
					</view>
					<view class="li">
						<text></text>
						<view>苹果手表</view>
					</view>
				</view>
			</view>
			<view class="ring"></view>
			<view id="btn">
				<image src="../../../../static/images/game/icon/finger.gif" alt="">
			</view>
		</view>
		<view class="border">
			10 EOTC 抽一次
		</view>
		<view class="balance">
			<view class="sq">
				<text>质押数量</text>
				<view id="zyje">100EOTC</view>
			</view>
			<view class="sq">
				<text>EOTC总量</text>
				<view id="integral">123</view>
			</view>
			<view class="sq">
				<text>质押收益</text>
				<view id="zysy">100EOTC</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		name: "turntable",
		data() {
			return {

			};
		}
	}
</script>

<style lang="scss" scoped>
	/*轮盘*/
	.main {
		position: relative;
	}

	.rotate {
		position: relative;
		width: 100%;
		height: 630rpx;
		top: -100rpx;

		.lunpai {
			position: absolute;
			left: 50%;
			top: 0;
			width: 86%;
			height: 100%;
			transform: translateX(-50%);
			background-image: url("../../../../static/images/game/image/circle.png");
			background-size: 100% 100%;
		}

		.ring {
			position: absolute;
			left: 50%;
			top: 14rpx;
			width: 82%;
			height: 96%;
			transform: translateX(-50%);
			// margin-left: -4.2rem;
			background-image: url("../../../../static/images/game/image/dark.png");
			background-size: 100% 100%;

			&.light {
				background-image: url("../../../../static/images/game/image/light.png");
			}
		}

		#btn {
			position: relative;
			left: 50%;
			top: 49%;
			width: 160rpx;
			height: 200rpx;
			// margin: -1.32rem 0 0 -1.06666667rem;
			transform: translate(-50%, -50%);
			background-image: url("../../../../static/images/game/image/button.png");
			background-size: 100% 100%;

			image {
				width: 100rpx;
				height: 100rpx;
				position: absolute;
				right: -50rpx;
				bottom: 34rpx;

			}
		}

		/*奖品*/
		.prize {
			overflow: hidden;
			position: absolute;
			left: 50%;
			top: 50%;
			transform: translate(-50%, -50%);
			margin: -10rpx 0 0 -2rpx;
			width: 88%;
			height: 89%;
			border-radius: 50%;

			// &.running {
			// 	-o-animation: init 90s linear infinite;
			// 	-ms-animation: init 90s linear infinite;
			// 	-moz-animation: init 90s linear infinite;
			// 	-webkit-animation: init 90s linear infinite;
			// 	animation: init 90s linear infinite;
			// }
			@keyframes init {
				100% {
					-o-transform: rotate(360deg);
					-ms-transform: rotate(360deg);
					-moz-transform: rotate(360deg);
					-webkit-transform: rotate(360deg);
					transform: rotate(360deg);
				}
			}

			.li {
				position: absolute;
				left: 0;
				top: 0;
				width: 100%;
				height: 288rpx;
				background: no-repeat center 35%;
				font-size: 25rpx;
				font-weight: bold;
				line-height: 70rpx;
				text-align: center;
				color: #e75228;
				-moz-transform-origin: center bottom;
				-webkit-transform-origin: center bottom;
				-o-transform-origin: center bottom;
				-ms-transform-origin: center bottom;
				transform-origin: center bottom;
				background-size: 80rpx 90rpx;

				&:nth-child(1) {
					background-image: url("../../../../static/images/game/image/EOTC.png");
					-o-transform: rotate(0deg);
					-ms-transform: rotate(0deg);
					-moz-transform: rotate(0deg);
					-webkit-transform: rotate(0deg);
					transform: rotate(0deg);
				}

				&:nth-child(2) {
					background-image: url("../../../../static/images/game/image/EOTCX.png");
					-o-transform: rotate(45deg);
					-ms-transform: rotate(45deg);
					-moz-transform: rotate(45deg);
					-webkit-transform: rotate(45deg);
					transform: rotate(45deg);
				}

				&:nth-child(3) {
					background-image: url("../../../../static/images/game/image/phone.png");
					-o-transform: rotate(90deg);
					-ms-transform: rotate(90deg);
					-moz-transform: rotate(90deg);
					-webkit-transform: rotate(90deg);
					transform: rotate(90deg);
				}

				&:nth-child(4) {
					background-image: url("../../../../static/images/game/image/USDT.png");
					-o-transform: rotate(135deg);
					-ms-transform: rotate(135deg);
					-moz-transform: rotate(135deg);
					-webkit-transform: rotate(135deg);
					transform: rotate(135deg);
				}

				&:nth-child(5) {
					background-image: url("../../../../static/images/game/image/computer.png");
					-o-transform: rotate(180deg);
					-ms-transform: rotate(180deg);
					-moz-transform: rotate(180deg);
					-webkit-transform: rotate(180deg);
					transform: rotate(180deg);
				}

				&:nth-child(6) {
					background-image: url("../../../../static/images/game/image/face.png");
					-o-transform: rotate(225deg);
					-ms-transform: rotate(225deg);
					-moz-transform: rotate(225deg);
					-webkit-transform: rotate(225deg);
					transform: rotate(225deg);
				}

				&:nth-child(7) {
					background-image: url("../../../../static/images/game/image/BOX.png");
					-o-transform: rotate(270deg);
					-ms-transform: rotate(270deg);
					-moz-transform: rotate(270deg);
					-webkit-transform: rotate(270deg);
					transform: rotate(270deg);
				}

				&:nth-child(8) {
					background-image: url("../../../../static/images/game/image/watch.png");
					-o-transform: rotate(315deg);
					-ms-transform: rotate(315deg);
					-moz-transform: rotate(315deg);
					-webkit-transform: rotate(315deg);
					transform: rotate(315deg);
				}

				text {
					position: absolute;
					top: 0;
					left: 0;
					width: 100%;
					height: 100%;
					margin-left: -50%;
					border-right: 2px solid #f6625c;
					-o-transform-origin: right bottom;
					-ms-transform-origin: right bottom;
					-moz-transform-origin: right bottom;
					-webkit-transform-origin: right bottom;
					transform-origin: right bottom;
					-o-transform: rotate(22.5deg) skewY(22.5deg);
					-ms-transform: rotate(22.5deg) skewY(22.5deg);
					-moz-transform: rotate(22.5deg) skewY(22.5deg);
					-webkit-transform: rotate(22.5deg) skewY(22.5deg);
					transform: rotate(22.5deg) skewY(22.5deg);
				}

				view {
					overflow: hidden;
					position: relative;
					margin: 0 auto;
					width: 50%;
					height: 60%;
					padding-top: 40rpx;
					text-align: center;
					line-height: 10rpx;
				}
			}

		}

	}

	/*次数*/
	.border {
		width: 60%;
		height: 65rpx;
		margin: -90rpx  0  40rpx 0;
		margin-left: 50%;
		transform: translateX(-50%);
		background: #fb3a13;
		font-size: 25rpx;
		line-height: 65rpx;
		text-align: center;
		color: #fff;
		border-radius: 35rpx;
	}

	.balance {
		display: flex;
		justify-content: space-around;
		font-size: 28rpx;

		.sq {
			width: 27%;
			height: 120rpx;
			padding: 10rpx;
			background: rgba(255, 255, 255, 0.5);
			border-radius: 20rpx;
			text-align: center;
			display: flex;
			flex-direction: column;
			justify-content: center;
			#integral {
				font-weight: bold;
				color: red;
			}
		}

		
	}
</style>
