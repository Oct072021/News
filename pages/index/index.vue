<template>
	<view>
		<scroll-view scroll-x class="navScroll">
			<view class="item" v-for="(item,index) in newsNav" :key="item.id" @click="onClick(index,item.id)"
				:class="navIndex==index ? 'activity' : '' ">
				{{ item.classname }}
			</view>
		</scroll-view>

		<view class="content">
			<view class="row" v-for="(item,index) in news" :key="item.id" @click="toDetails(item)">
				<newsBox :isAuthorShow="true" :content="item"></newsBox>
			</view>
		</view>

		<view class="none" v-if="!news.length">
			<image src="../../static/images/nodata.png" mode="widthFix"></image>
		</view>

		<view class="loading" v-if="news.length">
			<view v-if="loading==1">数据加载中...</view>
			<view v-if="loading==2">没有更多了~~</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				navIndex: 0,
				navId: 50,
				newsNav: [],
				news: [],
				currentPage: 1,
				loading: 0,
			}
		},
		onLoad() {
			this.getNewsNav()
			this.getNews()
		},
		onReachBottom() {
			this.currentPage++
			this.loading = 1
			this.getNews()
		},
		methods: {
			getNewsNav() {
				uni.request({
					url: "https://ku.qingnian8.com/dataApi/news/navlist.php",
					success: (res) => {
						this.newsNav = res.data
					}
				})
			},
			getNews() {
				uni.request({
					url: "https://ku.qingnian8.com/dataApi/news/newslist.php",
					data: {
						cid: this.navId,
						page: this.currentPage
					},
					success: (res) => {
						if (res.data.length <= 1) {
							this.loading = 2
						}
						this.news = [...this.news, ...res.data]
					}
				})
			},
			onClick(index, id) {
				this.navIndex = index
				this.navId = id
				this.currentPage = 1
				this.news = []
				this.getNews(id)
			},
			toDetails(e) {
				console.log(e)
				uni.navigateTo({
					url: `/pages/details/details?cid=${e.classid}&id=${e.id}`,
				})
			}
		}
	}
</script>

<style lang="scss" scoped>
	.navScroll {
		background-color: #F7F8FA;
		height: 100rpx;
		white-space: nowrap;
		position: fixed;
		top: var(--window-top);
		left: 0;
		z-index: 10;

		/deep/ ::-webkit-scrollbar {
			width: 4px !important;
			height: 1px !important;
			overflow: auto !important;
			background: transparent !important;
			-webkit-appearance: auto !important;
			display: block;
		}

		.item {
			font-size: 40rpx;
			line-height: 100rpx;
			display: inline-block;
			padding: 0 30rpx;

			&.activity {
				color: #31C27F;
			}
		}
	}

	.content {
		padding: 100rpx 30rpx 0;

		.row {
			padding: 20rpx 0;
			border: 1px dashed #efefef;
		}
	}

	.none {
		display: flex;
		justify-content: center;

		image {
			width: 360rpx;
		}
	}

	.loading {
		text-align: center;
		font-size: 26rpx;
		color: #cfcfcf;
		line-height: 2em;
	}
</style>
