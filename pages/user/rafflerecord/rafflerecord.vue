<template>
	<view class="wrap">
		<!--头部组件-->
		<u-navbar :is-back="true" :title="homeTitle" :background="background" :title-color="titleColor"
			:custom-back="about"></u-navbar>
			<u-image width="50%" height="240rpx" style="margin:30% auto 0;" mode="widthFix" src="../../../static/images/my/notdata.png"></u-image>
			    <view style="text-align:center;color:#999">暂无抽奖信息</view>
		<!-- <view class="" v-if="cardData.length>0">
			<u-card v-for="(item,index) in cardData" :key="item.gid" :title="item.type" title-size="50rpx">
				<view class="" slot="body">
					<view class="u-body-item u-flex u-col-between u-p-t-0 ">
						<view class="u-body-item-title u-line-2 list">
							<view>{{listdata.title1}}</view>
							<view>{{item.date}}</view>
						</view>
					</view>
					<view class="u-body-item u-flex u-col-between u-p-t-0 ">
						<view class="u-body-item-title u-line-2 list">
							<view>{{listdata.title2}}</view>
							<view>{{item.state}}</view>
						</view>
					</view>
					<view class="u-body-item u-flex u-col-between u-p-t-0 ">
						<view class="u-body-item-title u-line-2 list">
							<view>{{listdata.title3}}</view>
							<view v-if="item.state > 10 && item.address == ''" class="bg2">尚未领取</view>
							<view class="bg" v-else>已领取</view>
						</view>
					</view>
				</view>
			</u-card>
		</view>
		<u-empty mode="data" :margin-top="400"></u-empty>
		<u-loadmore :status="status" /> -->
		<!-- <u-back-top :scroll-top="scrollTop" top="600" ></u-back-top> -->
	</view>
</template>

<script>
	export default {
		data() {
			return {
				background: {
					backgroundColor: '#FFFFFF'
				},
				titleColor: '#333333',
				homeTitle: '抽奖记录',
				timer: null,
				timer2: null,
				scrollTop: 0,
				scrollTrigger: false,
				status: 'loading',
				listAll: 0, //信息总数
				listPage: 1, //请求页数
				listLoading: false, //是否正在加载
				listdata: {
					title1: "抽奖时间",
					title2: "奖品",
					title3: "领取状态",
				},
				cardData: []
			};
		},
		// onReady: () => {
		// 	uni.showLoading({
		// 		title: '加载中',
		// 	})
		// },
		// created() {
		// 	uni.request({
		// 		url: 'https://api.eotcyu.club/api/EOTC/EgameMsg', //抽奖记录
		// 		method: 'post',
		// 		data: {
		// 			token: localStorage.getItem('userToken')
		// 		},
		// 		header: {
		// 			"content-type": "application/x-www-form-urlencoded" //请求头信息
		// 		},
		// 		success: (res) => {
		// 			let text, textColor, event;
		// 			// console.log(res.data);
		// 			let result = res.data
		// 			if (result.length > 0) {
		// 				result = result.reverse();
		// 				for (let i = 0; i < result.length; i++) {
		// 					//游戏名字
		// 					if (result[i].type == '1') {
		// 						result[i].type = '幸运大转盘';
		// 						//领取状态
		// 						if (result[i].state > 10 && result[i].address == '') {
		// 							text = '尚未领取';
		// 							textColor = 'red';
		// 							event = `address(${result[i].gid})`;
		// 						} else {
		// 							text = '已领取';
		// 							textColor = 'green';
		// 							event = '';
		// 						}
		// 						if (result[i].state == 0) {
		// 							result[i].state = '谢谢参与~';
		// 						} else if (result[i].state == 1) {
		// 							result[i].state = '5 EOTC';
		// 						} else if (result[i].state == 2) {
		// 							result[i].state = '20 EOTC';
		// 						} else if (result[i].state == 13) {
		// 							result[i].state = '苹果13';
		// 						} else if (result[i].state == 4) {
		// 							result[i].state = '50USDT';
		// 						} else if (result[i].state == 12) {
		// 							result[i].state = '华为笔记本';
		// 						} else if (result[i].state == 3) {
		// 							result[i].state = 'NFT盲盒';
		// 						} else if (result[i].state == 11) {
		// 							result[i].state = '苹果手表';
		// 						}
		// 					} else if (result[i].type == '2') {
		// 						result[i].type = '砸金蛋';
		// 						text = '已领取';
		// 						textColor = 'green';
		// 						event = '';
		// 						if (result[i].state == 0) {
		// 							result[i].state = '谢谢参与~';
		// 						} else if (result[i].state == 1) {
		// 							result[i].state = '2 EOTC';
		// 						} else if (result[i].state == 2) {
		// 							result[i].state = '加速释放';
		// 						} else if (result[i].state == 3) {
		// 							result[i].state = 'NFT盲盒';
		// 						} else if (result[i].state == 4) {
		// 							result[i].state = '10 EOTC';
		// 						} else if (result[i].state == 5) {
		// 							result[i].state = '100 EOTC';
		// 						} else if (result[i].state == 6) {
		// 							result[i].state = '500 EOTC';
		// 						} else if (result[i].state == 7) {
		// 							result[i].state = '1000 EOTC';
		// 						} else if (result[i].state == 8) {
		// 							result[i].state = '500 USDT';
		// 						}
		// 					} else if (result[i].type == '3') {
		// 						result[i].type = '幸运翻卡';
		// 						//领取状态
		// 						if (result[i].state > 1 && result[i].address == '') {
		// 							text = '尚未领取';
		// 							textColor = 'red';
		// 							event = `address(${result[i].gid})`;

		// 						} else {
		// 							text = '已领取';
		// 							textColor = 'green';
		// 							event = '';
		// 						}
		// 						if (result[i].state == 0) {
		// 							result[i].state = '谢谢参与~';
		// 						} else if (result[i].state == 1) {
		// 							result[i].state = '1 EOTC';
		// 						} else if (result[i].state == 2) {
		// 							result[i].state = '智能音箱';
		// 						} else if (result[i].state == 3) {
		// 							result[i].state = '电动牙刷';
		// 						} else if (result[i].state == 4) {
		// 							result[i].state = '投影仪器';
		// 						} else if (result[i].state == 5) {
		// 							result[i].state = '华为耳机';
		// 						} else if (result[i].state == 6) {
		// 							result[i].state = '扫地机器人';
		// 						}
		// 					} else if (result[i].type == '4') {
		// 						result[i].type = '刮刮卡';
		// 						let val = Number(result[i].eotc)
		// 						text = '已领取';
		// 						textColor = 'green';
		// 						event = '';
		// 						if (result[i].state == 0) {
		// 							result[i].state = '谢谢参与~';
		// 						} else if (result[i].state == 1) {
		// 							result[i].state = `1 EOTC`;
		// 						} else if (result[i].state == 2) {
		// 							result[i].state = `${val*5} EOTC`;
		// 						} else if (result[i].state == 3) {
		// 							result[i].state = `${val*10} EOTC`;
		// 						} else if (result[i].state == 4) {
		// 							result[i].state = `${val*20} EOTC`;
		// 						} else if (result[i].state == 5) {
		// 							result[i].state = `${val*30} EOTC`;
		// 						} else if (result[i].state == 6) {
		// 							result[i].state = `${val*50} EOTC`;
		// 						} else if (result[i].state == 7) {
		// 							result[i].state = `${val*100} EOTC`;
		// 						}
		// 					} else if (result[i].type == '5') {
		// 						result[i].type = '抓福袋';
		// 						text = '已领取';
		// 						textColor = 'green';
		// 						event = '';
		// 						if (result[i].state == 0) {
		// 							result[i].state = '谢谢参与~';
		// 						} else if (result[i].state == 1) {
		// 							result[i].state = `10 EOTC`;
		// 						} else if (result[i].state == 2) {
		// 							result[i].state = `20 EOTC`;
		// 						} else if (result[i].state == 3) {
		// 							result[i].state = `500 EOTC`;
		// 						} else if (result[i].state == 4) {
		// 							result[i].state = `1000 EOTC`;
		// 						} else if (result[i].state == 5) {
		// 							result[i].state = `2000 EOTC`;
		// 						}
		// 					}
		// 				}
		// 			}
		// 			this.cardData = result
		// 			if (res.statusCode != 200) {
		// 				uni.hideLoading()
		// 				uni.showToast({
		// 					title: '加载失败',
		// 					duration: 2000,
		// 					icon: 'error',
		// 					complete: (res) => {
		// 						clearTimeout(this.timer)
		// 						this.timer = setTimeout(() => {
		// 							uni.hideToast()
		// 						}, 2000)
		// 						this.status = 'nomore'
		// 					}
		// 				})
		// 			} else {
		// 				uni.hideLoading()
		// 				uni.showToast({
		// 					title: '加载完成',
		// 					duration: 2000,
		// 					icon: 'success',
		// 					complete: (err) => {
		// 						clearTimeout(this.timer2)
		// 						this.timer2 = setTimeout(() => {
		// 							uni.hideToast()
		// 						}, 2000)
		// 						this.status = 'nomore'
		// 					}
		// 				})
		// 			}
		// 		}
		// 	});
		// },
		// // onLoad: function(options) {
		// // 	uni.startPullDownRefresh();
		// // },
		// onPullDownRefresh() {
		// 	clearTimeout(this.timer2)
		// 	this.timer2 = setTimeout(function() {
		// 		uni.stopPullDownRefresh();
		// 	}, 1000);
		// },
		// onPageScroll(e) {
		// 	this.scrollTop = e.scrollTop;
		// 	console.log(e.scrollTop, e, 'e.scrollTop')
		// },
		methods: {
			about() {
				uni.reLaunch({
					url: '/pages/user/index'
				})
			}
		}
	}
</script>

<style lang="scss" scoped>
	.u-card-wrap {
		background-color: $u-bg-color;
		padding: 1px;
	}

	.u-body-item {
		font-size: 32rpx;
		color: #333;
		padding: 20rpx 10rpx;
	}

	.wrap {
		height: 100%;
	}

	.list {
		display: flex;
		justify-content: space-between;
		align-items: center;
		width: 100%;
	}

	::v-deep .u-card__head--left__title[data-v-d7b36b00] {
		font-size: 32rpx;
		font-weight: bold;
	}

	.bg {
		color: #4DB978;
	}

	.bg2 {
		color: red;
	}
</style>
