<template>
	<view>
		<web-view :webview-styles="webviewStyles" :src="src"></web-view>
		<button @click="onlineReport">在线查看</button>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				webviewStyles: {
					progress: {
						color: '#FF3333'
					}
				},
				src:null
			};
		},
		methods:{
			onlineReport(){
				this.src="https://uniapp.dcloud.io/static/web-view.html"
			}
		}
	}
</script>

<style lang="scss">

</style>
