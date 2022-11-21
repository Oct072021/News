<template>
	<view>
		<view class="top">
			<image src="../../static/icon/history.png" mode="aspectFill"></image>
			<text>浏览历史</text>
		</view>

		<view class="content">
			<view class="row" v-for="item in historyArr" :key="item.id" @click="toDetails(item)">
				<newsBox :isLookTimeShow="true" :content="item"></newsBox>
			</view>
		</view>

		<view class="noHistory" v-if="!historyArr.length">
			<image src="../../static/images/nohis.png" mode="widthFix"></image>
			<text>暂无历史浏览记录</text>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				historyArr: []
			}
		},
		onShow() {
			this.getHistory()
		},
		methods: {
			toDetails(e) {
				uni.navigateTo({
					url: `/pages/details/details?cid=${e.classid}&id=${e.id}`,
				})
			},
			getHistory() {
				let his = uni.getStorageSync("historyArr") || []
				this.historyArr = his
			}
		}
	}
</script>

<style lang="scss">
	.top {
		padding: 50rpx 0;
		background-color: #F8F8F8;
		color: #555;
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;

		image {
			width: 150rpx;
			height: 150rpx;
		}

		text {
			font-size: 38rpx;
			padding-top: 20rpx;
		}
	}

	.content {
		padding: 30rpx;

		.row {
			padding: 20rpx 0;
			border: 1px dashed #efefef;
		}
	}

	.noHistory {
		display: flex;
		justify-content: center;
		flex-direction: column;
		align-items: center;

		image {
			width: 450rpx;
		}

		text {
			font-size: 26rpx;
			color: #888;
		}
	}
</style>
