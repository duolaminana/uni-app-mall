<template>
	<view class="newsList">
		<view class="news-item" v-for="item in newsList" :key="item.id" @click="navigate(item.id)">
			<image :src="item.img_url" mode=""></image>
			<view class="right">
				<view class="title">{{item.title}}</view>
				<view class="info">
					<text class="time">发表时间：{{item.add_time | showTime}}</text>
					<text class="count">浏览：{{item.click}}</text>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		props: {
			newsList: {
				type: Array,
				default: () => {
					return []
				}
			}
		},
		filters: {
			showTime(time) {
				return time.substr(0, 10)
			}
		},
		methods: {
			navigate(id) {
				this.$emit('itemClick', id)
			}
		}
	}
</script>

<style lang="scss" scoped>
	.newsList {
		.news-item {
			display: flex;
			padding: 10rpx 20rpx;
			border-bottom: 2rpx solid $shop-color;

			image {
				min-width: 200rpx;
				max-width: 200rpx;
				height: 150rpx;
			}

			.right {
				margin-left: 10rpx;
				display: flex;
				flex-direction: column;
				justify-content: space-between;

				.title {
					font-size: 30rpx;
				}

				.info {
					font-size: 24rpx;

					.time {
						margin-right: 30rpx;
					}
				}
			}
		}
	}
</style>
