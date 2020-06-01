<template>
	<view class="goods-list">
		<view class="good-item" v-for="item in goodsList" :key="item.id" @click="navigate(item.id)">
			<image :src="item.img_url" mode=""></image>
			<view class="price">
				<text class="sell">￥{{item.sell_price}}</text>
				<text class="firstSell">￥{{item.market_price}}</text>
			</view>
			<view class="name">{{item.title}}</view>
		</view>
	</view>
</template>

<script>
	export default {
		props: {
			goodsList: {
				type: Array,
				defult: () => {
					return []
				}
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
	.goods-list {
		display: flex;
		flex-wrap: wrap;
		justify-content: space-between;
		padding: 0 15rpx;

		.good-item {
			background-color: #fff;
			width: 350rpx;
			margin: 10rpx 0;
			padding: 15rpx;
			box-sizing: border-box;

			image {
				width: 80%;
				height: 300rpx;
				display: block;
				margin: auto;
			}

			.price {
				margin: 20rpx 0 5rpx 0;

				.sell {
					color: $shop-color;
					font-size: 36rpx;
				}

				.firstSell {
					color: #ccc;
					text-decoration: line-through;
					font-size: 28rpx;
					margin-left: 16rpx;
				}
			}

			.name {
				font-size: 28rpx;
				line-height: 50rpx;
				padding-top: 15rpx;
			}
		}
	}
</style>
