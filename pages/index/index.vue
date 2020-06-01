<template>
	<view class="home">
		<!-- 轮播图 -->
		<swiper class="swiper" indicator-dots autoplay indicator-active-color="#b50e03">
			<swiper-item class="swiper-item" v-for="(item,index) in swipers" :key="index">
				<navigator :url="item.url">
					<image :src="item.img" />
				</navigator>
			</swiper-item>
		</swiper>
		<!-- 导航介绍 -->
		<view class="nav">
			<view class="nav-item" v-for="(item,index) in navList" :key='index' @click="navItemClick(item.path)">
				<view :class="item.icon"></view>
				<view class="title">{{item.title}}</view>
			</view>
		</view>
		<!-- 推荐商品 -->
		<view class="hot-goods">
			<view class="title">推荐商品</view>
			<goods-list :goodsList="goodsList" @itemClick="goGoodsDetail" />
		</view>
		<view class="isShow" v-if="flag">------我是有底线的------</view>
	</view>
</template>

<script>
	import '../../static/iconfont/iconfont.css'
	import goodsList from '@/components/goodsList/goodsList.vue'
	export default {
		name: 'home',
		data() {
			return {
				flag: false,
				pageindex: 1,
				swipers: [],
				goodsList: [],
				navList: [{
						icon: 'iconfont icon-chaoshi',
						title: '购物超市',
						path: '/pages/goods/goods'
					},
					{
						icon: 'iconfont icon-lianxiwomen',
						title: '联系我们',
						path: '/pages/contact/contact'
					},
					{
						icon: 'iconfont icon-tupian',
						title: '社区图片',
						path: '/pages/pics/pics'
					},
					{
						icon: 'iconfont icon-shipin',
						title: '学习视频',
						path: '/pages/videos/videos'
					}
				]
			}
		},
		onLoad() {
			this.getSwipers()
			this.getGoods()
		},
		// 上拉触底事件
		onReachBottom() {
			if (this.goodsList.length < this.pageindex * 10) return this.flag = true
			this.pageindex++
			this.getGoods()
		},
		components: {
			goodsList
		},
		methods: {
			// 获取轮播图数据
			async getSwipers() {
				const res = await this.$request({
					url: '/api/getlunbo',
				})
				console.log(res)
				this.swipers = res.data.message
			},
			// 获取商品数据
			async getGoods() {
				const res = await this.$request({
					url: '/api/getgoods?pageindex=' + this.pageindex,
				})
				console.log(res)
				this.goodsList = [...this.goodsList, ...res.data.message]
			},
			// 导航点击事件
			navItemClick(url) {
				uni.navigateTo({
					url
				})
			},
			// 点击导航商品详情页
			goGoodsDetail(id) {
				uni.navigateTo({
					url: '/pages/goodsDetail/goodsDetail?id=' + id
				})
			}


		}
	}
</script>

<style lang="scss" scoped>
	.home {
		.swiper {
			width: 750rpx;
			height: 380rpx;

			image {
				width: 100%;
			}
		}

		.nav {
			display: flex;

			.nav-item {
				width: 25%;
				text-align: center;

				.iconfont {
					width: 120rpx;
					height: 120rpx;
					line-height: 120rpx;
					margin: 20rpx auto;
					border-radius: 50%;
					font-size: 50rpx;
					color: #fff;
					background-color: $shop-color;
				}

				.title {
					font-size: 30rpx;
				}
			}
		}

		.hot-goods {
			background-color: #eee;

			.title {
				height: 100rpx;
				line-height: 100rpx;
				text-align: center;
				color: $shop-color;
				font-size: 40rpx;
				letter-spacing: 40rpx;
				background-color: #fff;
				border-top: 0rpx solid #eee;
				border-bottom: 10rpx solid #eee;
			}
		}

		.isShow {
			width: 100%;
			height: 50rpx;
			line-height: 50rpx;
			text-align: center;
			font-size: 28rpx;
			background-color: #eee;
		}
	}
</style>
