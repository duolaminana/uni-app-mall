<template>
	<view class="contact">
		<image class="about" src="http://www.itcast.cn/2018czydz/images/gywmban.jpg" mode="" />
		<view class="info">
			<view class="phone" @click="phone">联系电话：400-618-9090 ( 点击拨打 )</view>
			<view class="address">校区地址：深圳市宝安区石岩海天蓝宇科技园2栋</view>
		</view>
		<map class="map" 
		:latitude="latitude" 
		:longitude="longitude" 
		:markers="cover"></map>
	</view>
</template>

<script>
	export default {
		name:'contact',
		data() {
			return {
				latitude:22.700595,
				longitude:113.980862,
				cover:[
					{
						latitude: 22.700595,
						longitude: 113.980862,
						iconPath: '../../static/logo.png',
						width:30,
						height:30
					}
				]
			}
		},
		methods: {
			phone(){
				uni.makePhoneCall({
				    phoneNumber: '400-618-9090' 
				});
			}
		}
	}
</script>

<style lang="scss" scoped>
	.contact{
		.about{
			width: 100%;
			height: 320rpx;
		}
		.info{
			padding: 10rpx 20rpx;
			font-size: 30rpx;
			line-height: 80rpx;
			view{
				border-bottom: 2rpx solid #eee;
			}
			.phone{
				
			}
			.address{
				font-size: 28rpx;
			}
		}
		.map{
			width: 750rpx;
			height: 750px;
			
		}
	}

</style>
