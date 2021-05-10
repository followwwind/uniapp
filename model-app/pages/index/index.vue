<template>
	<view class="padding bg">
		<view class="login_bg">
			<view class="flex  p-xs margin-bottom-sm mb-sm" >
				<view class="flex-sub padding-sm margin-xs radius login_font">用户名</view>
				<view class="flex-twice padding-sm margin-xs radius input">
					<input type="text" v-model="cjyxm" />
				</view>
			</view>
			<view class="flex  p-xs margin-bottom-sm mb-sm">
				<view class="flex-sub padding-sm margin-xs radius login_font">部队编号</view>
				<view class="flex-twice padding-sm margin-xs radius input">
					<input type="text" v-model="bdbh" />
				</view>
			</view>
			<view class="flex p-xs margin-bottom-sm mb-sm">
				<view class="flex-sub padding-sm margin-xs radius login_font">系统选择</view>
				<view class="flex-twice padding-sm margin-xs radius input">
					<input type="text" v-model="cjyxm" />
				</view>
			</view>
			<view class="flex p-xs margin-bottom-sm mb-sm">
				<view class="flex-sub padding-sm margin-xs radius" >
					<button @click="onLogin" class="login_no"></button>
				</view>
				<view class="flex-twice padding-sm margin-xs radius">
					<button @click="onRegister" class="reg_no"></button>
				</view>
			</view>
		</view>
		<view class="flex margin-bottom-sm">
			<view class="flex-sub radius">
				<button @click="onGetData" class="pull_no"></button>
			</view>
			<view class="flex-sub radius">
				<button @click="onSubmitData" class="submit_no"></button>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				title: 'Hello',
				cjyxm: '',
				bdbh: '',
				isGetData: false,
			}
		},
		onLoad() {

		},
		methods: {
			onLogin() {
				//登录,对比本地数据
				let cjyData = [{
					"id": 1,
					"createTime": null,
					"updateTime": null,
					"cjyxm": "采集员1",
					"bdbh": "228827",
					"xb": "男",
					"lxdh": "12322322232",
					"lxdz": "辽宁省沈阳市",
					"bz": "1"
				}, {
					"id": 2,
					"createTime": null,
					"updateTime": null,
					"cjyxm": "采集员2",
					"bdbh": "338828",
					"xb": "男",
					"lxdh": "22233312",
					"lxdz": "辽宁省沈阳市",
					"bz": "2"
				}];

				// getApp().fileWriter(getApp().globalData.cjyData,JSON.stringify(testData));
				//getApp().fileReader(getApp().globalData.cjyData); 
				
				if (!this.isGetData) {
					uni.showToast({
						title: '请先点击指标与数据表拉取',
						icon:"none",
						duration: 2000
					});
					this.cjyxm = '';
					this.bdbh = '';
					return;
				}


				//TODO 读取本地存储的文件
				let isUser = false;
				for (let i = 0; i < cjyData.length; i++) {
					if (this.cjyxm == cjyData[i].cjyxm && this.bdbh == cjyData[i].bdbh) {
						isUser = true;
						return;
					}
				}

				// if (!isUser) {
				// 	uni.showToast({
				// 		title: '账号有误',
				// 		icon:"none",
				// 		duration: 2000
				// 	});
				// 	this.cjyxm = '';
				// 	this.bdbh = '';

				// 	return;
				// }

				//跳转
				uni.navigateTo({
					url:"../modelselect/modelselect",
				});
			},
			onRegister() {
				//注册
				uni.navigateTo({
					url:"../register/register"  
				})
				
				
			},
			onGetData() {
				this.isGetData = true;
				uni.showToast({
					title: '拉取成功',
					icon:"none",
					duration: 2000
				});
				//拉取服务器数据
				let url = getApp().serverUrl + "/cjyxxbs/cjy";
				uni.request({
					url: url,
					success: (res) => {
						this.isGetData = true;
						//TODO 更新数据到本地，更新UI
						console.log(res);
					}
				})
			},
			onSubmitData() {
				//采集数据提交
			},
		}
	}
</script>

<style>
	.content {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
	}

	.logo {
		height: 200rpx;
		width: 200rpx;
		margin-top: 200rpx;
		margin-left: auto;
		margin-right: auto;
		margin-bottom: 50rpx;
	}

	.text-area {
		display: flex;
		justify-content: center;
	}

	.title {
		font-size: 36rpx;
		color: #8f8f94;
	}
	
	.login_font{
		color: white;
	}
	
	.login_bg{
		background: url(../../static/login/login.png);
		background-repeat: no-repeat;
		margin-top: 20%;
		padding-top: 120rpx;
	}
	
	.login_no{
		background-image: url(../../static/login/no_login.png);
		width: 200px;
		height: 72px;
	}
	
	.reg_no{
		background-image: url(../../static/login/no_reg.png);
		width: 200px;
		height: 72px;
	}
	
	.pull_no{
		background-image: url(../../static/login/no_pull.png);
		width: 400px;
		height: 80px;
	}
	
	.input{
		background-image: url(../../static/input/input.png);
	}
	
	.submit_no{
		background-image: url(../../static/login/no_submit.png);
		width: 400px;
		height: 80px;
		margin-left: 10px;
	}
</style>
