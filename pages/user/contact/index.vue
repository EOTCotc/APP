<template>
	<view class="contact_wrap">
        <u-navbar :is-back="true" back-icon-color="#333" title="联系我们" :custom-back="about"></u-navbar>
		<view class="main">
			<u-image class="logo" src="@/static/images/buy/logo.png" width="120" mode="widthFix"></u-image>
			<view class="name">EOTC</view>
			<view class="version">V2.0版本</view>
		</view>
		<view class="list">
			<u-cell-item title="官方网站" value="https://eotc.im"></u-cell-item>
			<u-cell-item title="官方电报" value="https://t.me/EOTOTC"></u-cell-item>
			<u-cell-item title="官方推特" value="https://twitter.com/EOTC9"></u-cell-item>
			<u-cell-item title="客服邮箱" value="coin@eotc.me" @click="showSheet()"></u-cell-item>
			<u-gap height="30" bg-color="#f5f6f7"></u-gap>
			<u-cell-item title="当前版本" value="V2.0"></u-cell-item>
		</view>
			<u-action-sheet :list="list" v-model="show" border-radius="25" @click="select"></u-action-sheet>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				list:[{text:"复制"},{text:"发送邮件"}],
				show:false,
			};
		},
		methods:{
			//返回
			about() {
				uni.reLaunch({
					url: '/pages/user/index'
				})
			},
			showSheet(){
				this.show=true;
			},
			select(index){
				console.log(index,`${this.list[index].text}`)
				
			}
		}
	}
</script>

<style scoped lang="scss">
	.contact_wrap {
		.main {
			padding: 80rpx 0;
			.logo {
				display: block;
				width: 116rpx;
				margin: 0 auto;
			}
			.name {
				color: #237FF8;
				font-size: 40rpx;
				margin-top: 6rpx;
				text-align: center;
			}
			.version {
				color: #999;
				font-size: 28rpx;
				margin-top: 20rpx;
				text-align: center;
			}
		}
		.list {
			background-color: #FFF;
		}
	}
</style>