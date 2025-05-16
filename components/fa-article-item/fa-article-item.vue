<template>
	<view class="list">
		<view class="fa-list-item u-border-bottom u-flex" v-for="(item, index) in archivesList" :key="index" @click="goDetail(item)">
			<view class="fa-item-image" v-if="!item.images_list.length">
				<image :src="item.image" mode="aspectFill"></image>
			</view>
			<view class="fa-item-content" :class="{ 'u-m-l-20': !item.images_list.length }">
				<view class="u-line-1 u-font-30 u-m-b-10 u-list-title" :style="[cmsTitleStyle(item.style)]">{{ item.title }}</view>
				<view class="u-flex u-flex-wrap u-m-t-20" v-if="item.images_list.length">
					<view class="images" v-for="(res, key) in item.images_list" :key="key">
						<u-image width="100%" border-radius="6" height="140" :src="res"></u-image>
					</view>
				</view>
				<view class="u-tips-color u-font-23 u-font-sm">时间：{{ item.create_date }}</view>
				<view class="article-tag u-flex">
					<view class="u-font-sm">
						<u-icon name="thumb-up-fill" color="#aaa" size="20"></u-icon>
						<text class="u-m-l-10 u-m-r-5">{{ item.likes }}</text>
					</view>
					<view class="u-m-l-30 u-font-sm">
						<u-icon name="chat-fill" color="#aaa" size="20"></u-icon>
						<text class="u-m-l-10 u-m-r-5">{{ item.comments }}</text>
					</view>
					<view class="u-m-l-30 u-font-sm">
						<u-icon name="eye-fill" color="#aaa" size="20"></u-icon>
						<text class="u-m-l-10 u-m-r-5">{{ item.views }}</text>
					</view>
				</view>
				<view class="u-tips-color u-font-23 u-font-sm">年收入：{{ formatIncome(item.likeratio) }}</view>
				<view class="u-m-b-10 u-font-23">
					<view class="u-red-wechat">
						<i class="iconfont icon-yuehui-mian"></i>
						红娘帮约
					</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
export default {
	props: {
		archivesList: {
			type: Array,
			default: []
		}
	},
	data() {
		return {};
	},
	methods: {
		goDetail(item) {
			let page = item.model_id == 2 ? '/pages/product/detail' : '/pages/article/detail';
			this.goPage(page + '?id=' + item.id);
		},
		// 计算年收入
		formatIncome(value) {
			const inWan = value / 10000;
			if (inWan % 1 === 0) {
				return inWan + '万';
			}
			return inWan.toFixed(2) + '万';
		}
	}
};
</script>

<style lang="scss">
.list {
	.fa-list-item {
		color: #333;
		margin: 30rpx;
		margin-top: 10rpx;
		margin-bottom: 15rpx;
		background: #fff;
		padding: 26rpx;
		border-radius: 20rpx;

		.fa-item-image {
			image {
				width: 275rpx;
				flex: 0 0 120rpx;
				height: 275rpx;
				border-radius: 20rpx;
				margin-right: 20rpx;
			}
		}

		.fa-item-content {
			width: 100%;

			.images {
				width: 31%;
				margin-bottom: 25rpx;
			}

			.article-tag {
				color: #aaa;
				font-size: 25rpx;
			}

			.u-list-title {
				color: #333333;
				font-size: 36rpx;
				font-weight: 600;
			}
		}
	}
}

.fa-item-content .images:nth-child(3n + 2) {
	margin: 0 3.5%;
}
</style>
