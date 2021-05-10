<template>
	<view class="padding bg">
		<view class="model-bg">
			<view v-for="(item, index) in modelList" :key="item.id" class="flex p-xs margin-bottom-sm mb-sm">
				<view class="flex-sub padding-sm margin-xs radius">
					<button @click="onSelectModel(item.id)" class="btn_bg">{{item.mxjs}}</button>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				modelList: '',
			}
		},
		methods: {
			onSelectModel(id) {
				console.log(id);
				//记录当前选择模型，跳转
				uni.navigateTo({
					url:"../record/record",
				})
			}

		},
		onShow: function() {
			console.log('modelselect Show');
			//TODO 加载模型数据
			let testData = [{
					"id": 1,
					"createTime": null,
					"updateTime": "2021-03-11 00:00:00",
					"mxm": "xxxx部队1",
					"mxjs": "是测试模型1",
					"mxtp": null,
					"updateUser": "1"
				},
				{
					"id": 2,
					"createTime": null,
					"updateTime": "2021-03-11 11:16:17",
					"mxm": "xxxx部队2",
					"mxjs": "是测试模型2",
					"mxtp": null,
					"updateUser": "1"
				}
			];
			this.modelList = testData;


		},
	}
</script>

<style>
	.model-bg{
		background: url(../../static/model/model.png) no-repeat;
		background-repeat: no-repeat;
		width: 100%;
		height: 600px;
		margin: 10% auto;
		padding-top: 150px;
	}
	
	.btn_bg{
		background-image: url(../../static/model/btn_bg.png);
		background-repeat: no-repeat;
		width: 80%;
		height: 135px;
		line-height: 135px;
		color: white;
		font-size: 20px;
		border: none;
		background-color: #031629;
	}
</style>
