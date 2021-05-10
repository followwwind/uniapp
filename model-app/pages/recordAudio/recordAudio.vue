<template>
	<view>
		<uni-nav-bar status-bar="true" title="录音界面" fixed="true" :shadow="true" backgroundColor="#81ecec"
			class="header"></uni-nav-bar>
		<lwj-record @sendRecList="getRecList" @delRecList="refreshRec($event, 1)" @delRecSingle="refreshRec($event, 2)"
			:containerHeight="canUseHeight + 'px'" :recordList="recordPathList"></lwj-record>
	</view>
</template>


<script>
	import lwjRecord from '@/components/lwj-record/lwj-record.vue';
	export default {
		data() {
			return {
				canUseHeight: 0,
				recordPathList: []
			};
		},
		components: {
			lwjRecord
		},
		mounted() {
			// 得到可用屏幕高度
			let {
				windowHeight
			} = uni.getSystemInfoSync();
			// 得到头部元素的高度
			let info = uni.createSelectorQuery().select('.header');
			info.boundingClientRect(el => {
				this.canUseHeight = windowHeight - el.height;
			}).exec();
		},
		methods: {
			/**
			 * @param {Object} latest
			 */
			getRecList(latest) {
				this.recordPathList.push(Object.assign({}, latest));
			},
			/**
			 * @param {Object} e
			 * @param {Number} mode
			 */
			refreshRec(e, mode) {
				// 当是批量删除时
				if (mode === 1) {
					// 得到被选中的列表
					let {
						chooseList
					} = e;
					// 刷新录音列表
					this.recordPathList = this.recordPathList.filter(item => {
						let canLeave = true;
						// 倘若选中项的路径等于数组中某个元素路径，那么需要被过滤掉
						chooseList.map(c => {
							if (c['src'] === item['src']) {
								canLeave = false;
							}
						});
						return canLeave;
					});
				}
				// 当单个删除时
				if (mode === 2) {
					// 得到被删除的录音
					let {
						item
					} = e;
					let needDelIndex = this.recordPathList.findIndex(r => {
						return item['src'] === r['src'];
					});
					this.recordPathList.splice(needDelIndex, 1);
				}
			}
		}
	};
</script>
