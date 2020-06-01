<template>
	<view class="news">
		<news-list :newsList="news" @itemClick="goNewsDetail"></news-list>
	</view>
</template>

<script>
	import newsList from '@/components/newsList/newsList.vue';
	export default {
		name: 'news',
		components: {
			newsList
		},
		data() {
			return {
				news: []
			}
		},
		methods: {
			// 获取资讯列表数据
			async getnewsList() {
				const res = await this.$request({
					url: '/api/getnewslist',
				})
				console.log(res)
				this.news = res.data.message
				uni.navigateTo({

				})
			},
			goNewsDetail(id) {
				uni.navigateTo({
					url: '/pages/newsDetail/newsDetail?id=' + id
				})
			}

		},
		onLoad() {
			this.getnewsList()
		},
	}
</script>

<style lang="scss" scoped>
	.news {}
</style>
