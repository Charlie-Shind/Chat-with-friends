<template>
	<view class="u-index-container">
		<!-- 顶部导航 -->
		<fa-navbar :title="vuex_table_title || '首页'" ref="navbar"></fa-navbar>
		<!-- 搜索 -->
		<!-- <view class="u-p-20 u-flex u-col-center" v-if="is_show">
			<view class="u-flex-1">
				<fa-search :mode="2"></fa-search>
			</view>
			<view class="u-p-l-15 u-p-r-5 u-flex u-col-center" v-if="is_order">
				<fa-orderby-select :filterList="filterList" :orderList="orderList" :multiple="true" @change="goOrderBy"></fa-orderby-select>
			</view>
		</view> -->
		<!-- 分类 -->
		<view class="u-border-top" v-if="isTab">
			<u-tabs :list="tabList" :active-color="theme.bgColor" :bar-width="tabwidth" name="title" :is-scroll="true" :current="current" @change="change"></u-tabs>
		</view>
		<!-- 轮播图 -->
		<view class="u-swiper-container" v-if="is_show">
			<u-swiper :title="true" border-radius="0" height="280" :list="bannerList" @click="openPage"></u-swiper>
		</view>
		<!-- 社区现状 -->
		<view class="u-community-status">
			<ul>
				<li>
					<img src="/static/image/man.png" alt="" />
					<view class="u-community-title">男嘉宾</view>
					<view class="u-community-num">1,929</view>
				</li>
				<li>
					<img src="/static/image/female.png" alt="" />
					<view class="u-community-title">女嘉宾</view>
					<view class="u-community-num">1,215</view>
				</li>
				<li>
					<img src="/static/image/newcomers.png" alt="" />
					<view class="u-community-title">本月新人</view>
					<view class="u-community-num">326</view>
				</li>
				<li>
					<img src="/static/image/singleness.png" alt="" />
					<view class="u-community-title">已脱单</view>
					<view class="u-community-num">220</view>
				</li>
			</ul>
		</view>
		<!-- 脱单喜报 -->
		<view class="u-singleness-news">
			<view class="u-singleness-box">
				<view class="subject">脱单<span class="u-bg_navs">喜报</span></view>
				<view class="body">
					<maoScroll :data="data" :showNum="showNum" :lineHeight="lineHeight" :animationScroll="animationScroll" :animation="animation">
						<template v-slot="{ line }">
							<view class="line">恭喜 {{ line.author }} 和 {{ line.subject }} 牵手成功</view>
						</template>
					</maoScroll>
				</view>
			</view>
		</view>
		<!-- 优质推荐 -->
		<view class="u-good-recommend">
			<span class="u-bg_navs">
				<img src="/static/image/recommend.png" alt="" />
			</span>
			优质推荐
		</view>
		<!-- 列表 -->
		<fa-article-item :archives-list="archivesList"></fa-article-item>
		<!-- 为空 -->
		<view class="u-m-t-60 u-p-t-60" v-if="is_empty">
			<u-empty text="暂无内容展示" mode="list"></u-empty>
		</view>
		<!-- 加载更多 -->
		<view class="u-p-30" v-if="archivesList.length">
			<u-loadmore bg-color="#f4f6f8" :status="status" />
		</view>
		<!-- 回到顶部 -->
		<u-back-top :scroll-top="scrollTop" :icon-style="{ color: theme.bgColor }" :custom-style="{ backgroundColor: lightColor }"></u-back-top>
		<!-- 底部导航 -->
		<fa-tabbar></fa-tabbar>
	</view>
</template>

<script>
import { archives } from '@/common/fa.mixin.js';
import maoScroll from '@/components/fa-mao-scroll/fa-mao-scroll.vue';

export default {
	components: {
		maoScroll
	},
	mixins: [archives],
	computed: {
		bannerList() {
			return this.vuex_config.bannerList || [];
		}
	},
	data() {
		return {
			title: 'Hello',
			data: [],
			count: 30,
			showNum: 1,
			lineHeight: 60,
			animationScroll: 500,
			animation: 2000
		};
	},
	onLoad(e) {
		let query = e;
		if (JSON.stringify(query) == '{}') {
			query = e;
		}
		if (query && JSON.stringify(query) != '{}') {
			this.params = query;
		} else {
			this.params = {
				channel: -1,
				model: -1
			};
		}
		this.getCategory();
		this.getArchives();
		console.log('init');
		console.log(this.data, '初始化数据 空数据');
		let self = this;
		setTimeout(function () {
			console.log('模拟从网上获取数据 花费了1秒');
			self.createData();
			console.log(self.data, '获取到的数据');
		}, 1000);
	},
	methods: {
		createData: function () {
			this.data.push({
				author: '2320**21',
				subject: '3211**12'
			});
		}
	}
};
</script>

<style lang="scss">
page {
	background-color: #f4f6f8;
}
</style>
