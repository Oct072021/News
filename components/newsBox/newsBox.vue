<template>
	<view class="newsBox">
		<view class="img">
			<image :src="content.picurl" mode="aspectFill"></image>
		</view>
		<view class="text">
			<view class="title">
				{{ content.title }}
			</view>
			<view class="sub_title" v-if="isAuthorShow">
				<text>{{ content.author }}</text>
				<text>{{ content.hits }}浏览</text>
			</view>
			<view class="sub_title" v-if="isLookTimeShow">
				<text>浏览时间：{{ content.looktime }}</text>
			</view>
		</view>
	</view>
	</view>
</template>

<script>
	export default {
		name: "newsBox",
		data() {
			return {

			}
		},
		props: {
			content: {
				type: Object,
				default () {
					return {
						title: "默认标题",
						author: "作者",
						hits: 668,
						picurl: "../../static/icon/nopic.png",
						looktime:"2022-11-16 18:46"
					}
				}
			},
			isAuthorShow:false,
			isLookTimeShow:false,
		}
	}
</script>

<style lang="scss">
	.newsBox {
		display: flex;

		.img {
			width: 230rpx;
			height: 160rpx;

			image {
				width: 100%;
				height: 100%;
			}
		}

		.text {
			flex: 1;
			padding-left: 20rpx;
			display: flex;
			flex-direction: column;
			justify-content: space-between;

			.title {
				font-size: 36rpx;
				color: #333;
				text-overflow: -o-ellipsis-lastline;
				overflow: hidden;
				text-overflow: ellipsis;
				display: -webkit-box;
				-webkit-line-clamp: 2;
				line-clamp: 2;
				-webkit-box-orient: vertical;
			}

			.sub_title {
				font-size: 26rpx;
				color: #999;
			}
			text{
				padding-right: 30rpx;
			}
		}
	}
</style>
