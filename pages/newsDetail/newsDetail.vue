<template>
	<view class="news-detail">
		<view class="detail-item" v-for="item in detail" :key="item.id">
				<view class="title">{{item.title}}</view>
				<view class="info">
					<text class="time">发表时间：{{item.add_time | showTime}}</text>
					<text class="see">浏览：{{item.click}}</text>
				</view>
				<view class="content">
					<rich-text :nodes="item.content" selectable></rich-text>
				</view>
			</view>
	</view>
</template>

<script>
	export default {
		name:'news-detail',
		data() {
			return {
				detail:[],
				id:null,
			}
		},
		filters:{
			showTime(time){
				return time.substr(0,10)
			}
		},
		 onLoad(option) {
			console.log(option)
			this.id = option.id
			this.getDetail()
		},
		methods: {
			async getDetail(){
				const res = await this.$request({
					url:'/api/getnew/'+this.id
				})
				console.log(res)
				this.detail = res.data.message
			}
		}
	}
</script>

<style lang="scss" scoped>
	.news-detail{
		padding: 10rpx;
		.detail-item{
			.title{
				font-size: 40rpx;
				line-height: 60rpx;
			}
			.info{
				font-size: 26rpx;
				margin: 20rpx 0;
				border-bottom: 2rpx solid $shop-color;
				line-height: 70rpx;
				.time{
					margin-right: 40rpx;
				}
			}
			.content{
				font-size: 30rpx;
			}
		}
	}
</style>
