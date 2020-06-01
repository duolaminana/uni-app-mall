<template>
	<view class="goods-detail">
		<!-- 轮播图 -->
		<swiper class="swiper" indicator-dots autoplay indicator-active-color="#b50e03">
			<swiper-item class="swiper-item" v-for="(item,index) in thumimages" :key="index">
				<image :src="item.src" />
			</swiper-item>
		</swiper>
		<!-- 简介 -->
		<view class="desc">
			<view class="box1">
				<view class="price">
					<text class="sell">￥{{info.sell_price}}</text>
					<text class="firstSell">￥{{info.market_price}}</text>
				</view>
				<view class="name">
					{{info.title}}
				</view>
			</view>
			<view class="line"></view>
			<view class="box2">
				<view class="goodsNum">货号：{{info.goods_no}}</view>
				<view class="stock">库存：{{info.stock_quantity}}</view>
			</view>
			<view class="line"></view>
		</view>
		<!-- 详情介绍 -->
		<view class="descMore">
			<view class="text">详情介绍</view>
			<view class="content">
				<rich-text :nodes="desc.content"></rich-text>
			</view>
		</view>
		<!-- 导航栏 -->
		<view class="nav">
			<uni-goods-nav :fill="true" :options="options" :buttonGroup="buttonGroup" @click="onClick" @buttonClick="buttonClick" />
		</view>
	</view>
</template>

<script>
	import uniGoodsNav from '@/components/uni-goods-nav/uni-goods-nav.vue'
	export default {
		name: 'goodsDetail',
		components: {
			uniGoodsNav
		},
		data() {
			return {
				id: null,
				thumimages: [],
				info: null,
				desc: null,
				options: [{
					icon: 'headphones',
					text: '客服'
				}, {
					icon: 'shop',
					text: '店铺',
				}, {
					icon: 'cart',
					text: '购物车',
					info: 2
				}],
				buttonGroup: [{
						text: '加入购物车',
						backgroundColor: '#ff0000',
						color: '#fff'
					},
					{
						text: '立即购买',
						backgroundColor: '#ffa200',
						color: '#fff'
					}
				]
			}
		},
		methods: {
			// nav
			onClick(e) {
				console.log(e)
				uni.showToast({
					title: `点击${e.content.text}`,
					icon: 'none'
				})
			},
			buttonClick(e) {
				console.log(e)
				this.options[2].info++
			},

			// 获取商品详情轮播图
			async getthumimages() {
				const res = await this.$request({
					url: '/api/getthumimages/' + this.id
				})
				console.log(res)
				this.thumimages = res.data.message
			},
			// 获取商品详情参数
			async getinfo() {
				const res = await this.$request({
					url: '/api/goods/getinfo/' + this.id
				})
				console.log(res)
				this.info = res.data.message[0]
			},
			// 获取商品详情介绍
			async getdesc() {
				const res = await this.$request({
					url: '/api/goods/getdesc/' + this.id
				})
				console.log(res)
				this.desc = res.data.message[0]
			}

		},
		onLoad(option) {
			console.log(option)
			this.id = option.id
			this.getthumimages()
			this.getinfo()
			this.getdesc()

		}
	}
</script>

<style lang="scss" scoped>
	.goods-detail {
		.swiper {
			height: 700rpx;

			image {
				width: 100%;
				height: 100%;
			}
		}

		.desc {
			margin-top: 20rpx;

			.line {
				height: 10rpx;
				width: 750rpx;
				background: #eee;
			}

			.box1 {
				padding: 0 20rpx;
				margin-bottom: 10rpx;
				line-height: 60rpx;

				.price {
					.sell {
						color: $shop-color;
						font-size: 40rpx;
					}

					.firstSell {
						color: #ccc;
						text-decoration: line-through;
						font-size: 28rpx;
						margin-left: 16rpx;
					}
				}

				.name {
					font-size: 30rpx;
					margin-bottom: 20rpx;

				}
			}

			.box2 {
				padding: 0 20rpx;
				line-height: 60rpx;
			}
		}

		.descMore {
			.text {
				padding: 0 20rpx;
				font-size: 34rpx;
				line-height: 80rpx;
				border-bottom: 2rpx solid #eee;
			}

			.content {
				margin-top: 20rpx;
				font-size: 28rpx;
				line-height: 50rpx;
				margin-bottom: 50rpx;

				/deep/ .gomeImgLoad {
					width: 750rpx;
					height: auto;
				}
			}
		}

		.nav {
			position: fixed;
			bottom: 0;
			width: 100%;
		}
	}
</style>
