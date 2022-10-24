<template>
	<u-row class="tarbar_wrap">
		<u-col class="item" :span="3" v-for="(item, index) in list" :key="index" @click="go(item.path, index, item.type)">
			<view class="wrap" :class="{'active': myActive === index}">
				<image class="img" :src="`${item.icon}${myActive === index ? '_active' : ''}.png`" mode="widthFix"></image>
				<view class="text">{{item.name}}</view>
			</view>
		</u-col>
	</u-row>
</template>

<script>
	export default {
		props:{
			active:{
				type:Number,
				default:0
			}
		},
		data() {
			return {
				  myActive : this.active,
				list: [
					{
						path: '/pages/index/default/default',
						name: '币秋',
						icon: '/static/images/common/home',
						type: 'reLaunch'
					},
					{
						path: '/pages/index/default/shopHome',
						name: 'E商城',
						icon: '/static/images/common/shop',
						type: 'reLaunch'
					},
					{
						path: '/pages/game/index',
						name: '游戏',
						icon: '/static/images/common/game',
						type: 'reLaunch'
					},
					{
						path: '/pages/user/index',
						name: '我的',
						icon: '/static/images/common/user',
						type: 'reLaunch'
					}
				]
			}
		},
		methods: {
			go(path, index, type) {
				console.log(index)
				this.myActive = index
				console.log(type, path)
				uni[type]({
					url: path
				})
			}
		}
	}
</script>

<style scoped lang="scss">
	.tarbar_wrap { position: fixed; left: 0; bottom: 0; width: 100%; background-color: #FFF;
		.item { height: 100rpx;
			.wrap { display: flex; flex-direction: column; justify-content: center; align-items: center; height: 100%;
				.img { width: 40rpx; height: 40rpx; }
				.text { font-size: 24rpx; color: #999; }
				&.active {
					.text { color: #333; }
				}
			}
		}
	}
</style>