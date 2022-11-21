<template>
	<view>
		<view class="detail">
			<view class="title">{{ details.title }}</view>
			<view class="info">
				<view class="author">编辑：{{ details.author }}</view>
				<view class="time">{{ details.posttime }}</view>
			</view>
			<view class="content">
				<rich-text :nodes="details.content"></rich-text>
			</view>
			<view class="descriptions">
				声明：本站内容均采集于网络，如有侵权请联系管理（1299424192@qq.com）进行整改删除，本站采集的内容不代表本站及作者观点，若有侵犯请联系管理！
			</view>
		</view>
	</view>
</template>

<script>
	import {
		parseTime
	} from '../../utils/tool'

	export default {
		data() {
			return {
				details: {}
			}
		},
		onLoad(option) {
			this.getNewsDetails(option.cid, option.id)
		},
		methods: {
			getNewsDetails(cid, id) {
				uni.request({
					url: "https://ku.qingnian8.com/dataApi/news/detail.php",
					data: {
						cid: cid,
						id: id
					},
					success: (res) => {
						res.data.posttime = parseTime(res.data.posttime)
						res.data.content = res.data.content.replace(/<img/gi, '<img style="max-width:100%"')
						this.details = res.data
						this.saveHistory()
					}
				})
			},
			saveHistory() {
				let historyArr = uni.getStorageSync("historyArr") || []
				let history = {
					id: this.details.id,
					classid: this.details.classid,
					picurl: this.details.picurl,
					title: this.details.title,
					looktime: parseTime(Date.now())
				}

				let index = historyArr.findIndex(i => {
					return i.id == history.id
				})

				if (index >= 0) {
					historyArr.splice(index, 1)
				}

				historyArr.unshift(history)
				historyArr = historyArr.slice(0, 10)
				uni.setStorageSync("historyArr", historyArr)
			}
		}
	}
</script>

<style lang="scss">
	.detail {
		padding: 30rpx;

		.title {
			font-size: 46rpx;
			color: #000000;
		}

		.info {
			background-color: #F6F6F6;
			padding: 20rpx 10rpx;
			font-size: 25rpx;
			color: #a6a6a6;
			display: flex;
			justify-content: space-between;
			margin: 40rpx 0;
		}

		.content {
			padding-bottom: 50rpx;
		}

		.descriptions {
			background: #FEF0F0;
			font-size: 26rpx;
			padding: 20rpx;
			color: #F89898;
			line-height: 1.8em;
		}
	}
</style>
