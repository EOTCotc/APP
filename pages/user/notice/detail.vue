<template>
	<view class="notice-detail_wrap">
        <u-navbar :is-back="true" back-icon-color="#333" :title="list.title"></u-navbar>
		<u-gap height="16" bg-color="#F5F6F7"></u-gap>
		<view class="main">
			<u-parse :html="list.content"></u-parse> 
		<!-- 	<text class="text">
				{{list.content}}
			</text> -->
			<view class="team">{{list.creatorName}}</view>
			<view class="date">{{list.createDate|timeFormat('yyyy年mm月dd日')}}</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				id:'',
				list:{
					  title: "欢庆国庆奖励活动",
					  content: "尊敬的EOTC会员：<br/>为庆祝EOTC swap聚合币币交易所正式上线,EOTC会员系统给新注册用户、老用户增加质押、新用户第一次质押惊喜奖励。<p>1、新用户注册，给予用户和推荐人各空投150枚EOTC代币</p><p>2、新用户首次质押，给予额外赠送30%EOTC代币</p><p>3、老用户增加质押，给予额外赠送30%EOTC代币</p><p>4、本次活动从2022年10月8日00：00开始至2022年10月24日23：59结束以上奖励方案系统自动执行。</p>",
					  creatorName: "EOTC技术团队",
					 noticeId: "8b6f6dfb-6abf-436f-80b0-0bc0c5beecaf",
					 createDate:"2022-10-20T01:55:59.342Z",
					 isDelete: 0
					}
			}
		},
		onLoad(option) {
				this.id = option.id;
		},
		created() {
			this.$u.api.noticebyId({id:this.id}).then(res=>{
				// this.list=res.items;
				console.log(this.list)
			})
		}
	}
</script>

<style scoped lang="scss">
	.notice-detail_wrap {
		background-color: #FFF;
		min-height: 100vh;
		.main {
			padding: 30rpx;
			color: #333;
			font-size: 32rpx;
			line-height: 56rpx;
			.team {
				text-align: right;
				margin-top: 40rpx;
			}
			.date {
				text-align: right;
				margin-top: 20rpx;
			}
		}
	}
</style>