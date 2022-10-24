<template>
	<view>
		<!--头部组件-->
		<u-navbar :is-back="false" :title="homeTitle" :background="background" :title-color="titleColor" ></u-navbar>
		<view  >
			<u-image width="50%" height="400rpx" style="margin:30% auto 0;" mode="widthFix" src="../../../static/images/my/not.png"></u-image>
			<view style="text-align: center;margin-right:35rpx;">暂未开放 敬请期待</view>
		</view>
		<!-- 底部组件 -->
		<biqiu-tarbar  :active="active"></biqiu-tarbar>
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
				homeTitle:"商城",
				active:1,
				background: {
					backgroundColor: '#FFFFFF'
				},
				titleColor: '#333333',
			}
		},
		methods: {
		}
	}
</script>

<style>

</style>
