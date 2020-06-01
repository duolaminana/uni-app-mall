<template>
	<view class="goods">
		<goods-list :goodsList="goodsList" @itemClick="goGoodsDetail" />
		<view class="isShow" v-if="flag">------我是有底线的------</view>
	</view>
</template>

<script>
	import goodsList from '@/components/goodsList/goodsList.vue'
	export default {
		data() {
			return {
				pageindex:1,
				goodsList:[],
				flag:false
			}
		},
		components:{
			goodsList
		},
		onLoad() {
			this.getGoods()
		},
		// 上拉触底事件
		onReachBottom(){
			if(this.goodsList.length<this.pageindex*10) return this.flag = true
			this.pageindex++
			this.getGoods()
		},
		// 下拉刷新
		onPullDownRefresh(){
			console.log('下拉刷新')
			this.pageindex = 1;
			this.flag = false;
			this.goodsList = [];
			setTimeout(() => {
				this.getGoods()
				uni.stopPullDownRefresh()
			},1000)
		},
			
		methods: {
			// 获取商品数据
			async getGoods(){
				const res = await this.$request({
					url:'/api/getgoods?pageindex='+this.pageindex,
							})
				console.log(res)
				this.goodsList = [...this.goodsList,...res.data.message]
			},
			
			goGoodsDetail(id){
				uni.navigateTo({
					url: '/pages/goodsDetail/goodsDetail?id=' + id
				})
			}	
		}
	}
</script>

<style lang="scss" scoped>
	.goods{
		background-color: #eee;
		.isShow{
			width: 100%;
			height: 50rpx;
			line-height: 50rpx;
			text-align: center;
			font-size: 28rpx;
			background-color: #eee;
		}
	}
</style>
