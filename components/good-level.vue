<template>
	<view class="good-tag-level">
		<view class="level1" v-if="level === 0">初级</view>
		<view class="level2" v-if="level === 1">中级</view>
		<view class="level3" v-if="level === 2">高级</view>
	</view>
</template>

<script>
	export default {
		props: {
			level: {
				type: Number,
				default: 1
			}
		}
	}
</script>

<style scoped lang="scss">
	.good-tag-level { width: 112rpx; height: 40rpx; line-height: 40rpx; font-size: 24rpx; text-align: center; border-radius: 30rpx; border-radius: 40px; overflow: hidden;
		.level1 { color: $color1; background-color: $bg-color1; }
		.level2 { color: $color2; background-color: $bg-color2; }
		.level3 { color: $color3; background-color: $bg-color3; }
	}
</style>