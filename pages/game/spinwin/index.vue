<template>
	<view class="">
		<web-view :src="src" fullscreen :webview-styles="webviewStyles"></web-view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				webviewStyles: {
					width: '100%',
					height: '100%'
				},
				src: null,
				canBack: false // 判断是否有回退页面
			}
		},
		onLoad() {
			this.src = "https://www.baidu.com"
			console.log(this.src, 'this.src')
			// #ifdef APP-PLUS
			const _this = this
			const curr = this.$scope.$getAppWebview().children()[0] // 获取当前webview
			setTimeout(function() {
				curr.addEventListener('progressChanged', function(e) {
					curr.canBack(function(e) {
						_this.canBack = e.canBack
					})
				})
			}, 500)
			// #endif
		},
		// 自定义返回键事件
		onBackPress(e) {
			// #ifdef APP-PLUS
			if (this.canBack) {
				this.$scope.$getAppWebview().children()[0].back() // 返回上一页
			} else {
				plus.runtime.quit() // 直接退出应用
			}
			return true
			// #endif
		}
	}
</script>

<style lang="scss" scoped>
</style>
