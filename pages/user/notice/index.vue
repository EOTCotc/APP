<template>
	<view class="notice_wrap">
        <u-navbar :is-back="true" back-icon-color="#333" title="公告"  :custom-back="about"></u-navbar>
		<view class="list" v-for="item in noticeArr" @click="notice_detail(item.noticeId)">
			<view class="item">
				<view class="header">{{item.title}}</view>
				<u-line color="#F5F6F7" :hair-line="false" />
				<view class="text u-line-1">{{item.content}}</view>
				<view class="flex">
					<text class="team">{{item.creatorName}}</text>
					<text class="date">{{item.createDate|timeFormat('yyyy年mm月dd日')}}</text>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				noticeArr:[
					{
					  title: "欢庆国庆奖励活动",
					  content: "尊敬的COTC会员为庆祝COTC swap聚合币币交易所正式上线，EOTC会员系统给新注册用户，老用户增加质押、新用户第一次质押惊喜奖励。",
					  creatorName: "EOTC技术团队",
					   noticeId: "9b6f6dfb-6abf-436f-90b0-0bc0c5beecaf",
					        createDate:"2022-10-20T01:55:59.342Z",
					        isDelete: 0
					},
					{
					  title: "欢庆国庆奖励活动111",
					  content: "尊敬的COTC会员为庆祝COTC swap聚合币币交易所正式上线，EOTC会员系统给新注册用户，老用户增加质押、新用户第一次质押惊喜奖励。",
					  creatorName: "EOTC技术团队",
					 noticeId: "8b6f6dfb-6abf-436f-80b0-0bc0c5beecaf",
					 createDate:"2022-10-20T01:55:59.342Z",
					 isDelete: 0
					}
				]
			}
		},
		created() {
			this.$u.api.getNotice().then(res=>{
				// this.noticeArr=res.items;
					// console.log(this.noticeArr)
			})
		},
		methods:{
			notice_detail(id){
				uni.navigateTo({
					url: `/pages/user/notice/detail?id=${id}`
				})
			},
			about(){
				uni.reLaunch({
					url: '/pages/user/index'
				})
			}
		}
	}
</script>

<style scoped lang="scss">
	.notice_wrap {
		.list {
			padding: 3px 30rpx 6rpx;
			.item {
				padding: 0 30rpx;
				border-radius: 20rpx;
				background-color: #FFF;
				margin-top: 30rpx;
				.header {
					color: #333;
					padding: 30rpx 0;
					font-size: 32rpx;
					font-weight: bold;
				}
				.text {
					color: #333;
					padding: 30rpx 0;
					font-size: 32rpx;
					line-height: 56rpx;
				}
				.flex {
					display: flex;
					justify-content: space-between;
					align-items: center;
					padding-bottom: 30rpx;
				}
				.team, .date {
					color: #999;
					font-size: 28rpx;
				}
			}
		}
	}
</style>