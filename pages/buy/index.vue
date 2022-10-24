<template>
	<view class="container">
		<u-toast ref="uToast" />
		<u-no-network></u-no-network>
		<u-navbar title-bold back-icon-color='#fff' title='购买' title-color='#fff' :background='{background:"#0A0E29"}'></u-navbar>
		<img class='index_buy' src="@/static/images/buy/index_buy.png" alt="">
		<view class="content">
			<view class="title">全球首个区块链缠论指标系统</view>
			<view class="account-box" >
				<view class="account" v-for="item,index in arr"  :class="{selected:isSelected}"  @click="handleSelected(index)">
					<view v-if="item.type==0">1个月</view>
					<view v-if="item.type==1">12个月</view>
					<view>
						<span>{{item.price}}</span>
						<span>USDT</span>
					</view>
					<view>{{item.name}}</view>
				</view>
				<!-- <view class="account" :class="{selected:isSelected}" @click="handleSelected">
					<view>12个月</view>
					<view>
						<span>10000</span>
						<span>USDT</span>
					</view>
					<view>{{item.name}}</view>
				</view> -->
			</view>
		</view>
		<view class="buy">
			<button @click="btn_buy()">购买</button>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				isSelected: false,
				// month:"",//月
				// jifen:"",//积分
				arr:[]
			}
		},
		created() {
			this.$u.api.getClsystem().then(res=>{
				this.arr=res.items;
				this.arr.map(item=>{
					this.$set(item,'isSelected',false)
				})
				console.log(this.arr)
			})
		},
		methods: {
			handleSelected(index) {
				this.arr[index].isSelected = true;
				this.isSelected=this.arr[index].isSelected;
								// this.list.map((val, idx) => {
								// 	if (index != idx) this.list[idx].show = false;
								// })

				// this.isSelected=isSelected
				// if(this.isSelected1){
				// 	this.month=1;
				// 	this.jifen=1000;
				// }else if(this.isSelected2){
				// 	this.month=12;
				// 	this.jifen=10000;
				// }
			},
			btn_buy(){
					console.log()
					uni.navigateTo({
						url: `/pages/subOrder/index?month=${this.month}&total=${this.jifen}`
					})
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
