<template>
	<view class="pics">
		<scroll-view class="left" scroll-y>
			<van-sidebar 
			@change="onChange(item.id,index)" 
			v-for="(item,index) in cates" :key="item.id"
			 >
			  <van-sidebar-item 
			  :class="active===index?'active':''"
			  :title="item.title" />
			</van-sidebar>
		</scroll-view>
		<view class="right">
			<view class="cateMore-item" 
			v-for="item in cateMore" :key="item.id">
				<image :src="item.img_url" @click="imgClick(item.img_url)" />
				<view class="title">{{item.title}}</view>
			</view>
		</view>
		
	</view>
</template>

<script>
	export default {
		name:'pics',
		data() {
			return {
				active:0,
				activeKey:null,
				cates:[],
				cateMore:[],
			}
		},
		methods: {
			// 切换栏切换变化
				async onChange(id,num){
					console.log(num,id)
					this.active = num
					// 获取分类详细数据
					const res = await this.$request({
						url:'/api/getimages/'+id,
						})
					console.log(res)
					this.cateMore = res.data.message
				},
			// 获取图片分类
				async getPicsCate(){
					const res = await this.$request({
						url:'/api/getimgcategory',
						})
					console.log(res)
					this.cates = res.data.message
					this.onChange(this.cates[0].id,0)
				},
			// 点击图片预览
			imgClick(current){
				const urls = this.cateMore.map(item => {
					return item.img_url
				})
				uni.previewImage({
					urls,
					current
				})
			}
		},
		onLoad() {
			this.getPicsCate()
		},
	}
</script>

<style lang="scss" scoped>
	.pics{
		display: flex;
		margin-top: 2rpx;
		.left{
			/deep/ .van-sidebar{
				width: 210rpx;
				text-align: center;

			}
			/deep/ .van-sidebar-item{
				border-left:0
			}
			.active{
				/deep/ .van-sidebar-item{
					background-color: $shop-color;
					color: #fff;
				}
				
			}	
		}
		.right{
			padding: 10rpx;
			width: 520rpx;
			image{
				width: 520rpx;
				border-radius: 10rpx;
			}
			.title{
				font-size: 30rpx;
				line-height: 60rpx;
			}
			
			
		}
	}

</style>
