<template>
	<view class="maoScroll-main" :style="'height:' + lineHeight * showLine + 'rpx;'">
		<view
			class="scroll-content"
			:style="{
				marginTop: -currentOffset + 'rpx',
				transition: isAnimating ? 'margin-top ' + animationScroll + 'ms linear' : 'none'
			}"
			@transitionend="onTransitionEnd"
		>
			<view v-for="(item, index) in showdata" :key="'scroll-' + index" :style="'height:' + lineHeight + 'rpx;'">
				<slot :line="item" />
			</view>
		</view>
	</view>
</template>

<script>
export default {
	name: 'maoScroll',
	props: {
		data: {
			type: Array,
			default: () => []
		},
		showNum: {
			type: Number,
			default: 3
		},
		lineHeight: {
			type: Number,
			default: 60
		},
		animationScroll: {
			type: Number,
			default: 500
		},
		animation: {
			type: Number,
			default: 2000
		}
	},
	data() {
		return {
			showdata: [],
			showLine: 0,
			currentIndex: 0,
			isAnimating: false,
			timer: null,
			currentOffset: 0
		};
	},
	methods: {
		init() {
			clearInterval(this.timer);
			this.showLine = Math.min(this.showNum, this.data.length);
			this.showdata = [...this.data, ...this.data.slice(0, this.showLine)];
			this.currentIndex = 0;
			this.currentOffset = 0;
			this.startLoop();
		},
		startLoop() {
			this.timer = setInterval(() => {
				this.scrollNext();
			}, this.animation);
		},
		scrollNext() {
			this.isAnimating = true;
			this.currentIndex++;
			this.currentOffset = this.currentIndex * this.lineHeight;
		},
		onTransitionEnd() {
			if (this.currentIndex >= this.data.length) {
				// 滚动到了末尾后，马上重置
				this.isAnimating = false;
				this.currentIndex = 0;
				this.currentOffset = 0;
			}
		}
	},
	watch: {
		data() {
			this.init();
		}
	},
	mounted() {
		this.init();
	},
	beforeDestroy() {
		clearInterval(this.timer);
	}
};
</script>

<style>
.maoScroll-main {
	width: 100%;
	overflow: hidden;
}
</style>
