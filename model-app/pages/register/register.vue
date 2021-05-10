<template>
	<view class="padding bg">
		<view class="reg_bg">
			<view class="flex  p-xs margin-bottom-sm mb-sm">
				<view class="flex-sub reg_font padding-sm margin-xs radius">
					<text>采集员姓名</text>
				</view>
				<view class="flex-twice input padding-sm margin-xs radius">
					<input type="text" v-model="cjyxm" />
				</view>
			</view>
			<view class="flex  p-xs margin-bottom-sm mb-sm">
				<view class="flex-sub reg_font padding-sm margin-xs radius">
					<text>部队编号</text>
				</view>
				<view class="flex-twice input padding-sm margin-xs radius">
					<input type="text" v-model="bdbh" />
				</view>
			</view>
			<view class="flex  p-xs margin-bottom-sm mb-sm">
				<view class="flex-sub reg_font padding-sm margin-xs radius">
					<view class="uni-title">性别</view>
				</view>
				<view class="flex-twice input padding-sm margin-xs radius">
					<view class="uni-padding-wrap">
						<view>
							<radio-group @change="selectSex">
								<label class="radio">
									<radio value="男" checked="true" />男
								</label>
								<label class="radio">
									<radio value="女" />女
								</label>
							</radio-group>
			
						</view>
					</view>
				</view>
			</view>
			<view class="flex  p-xs margin-bottom-sm mb-sm">
				<view class="flex-sub reg_font padding-sm margin-xs radius">
					<text>联系电话</text>
				</view>
				<view class="flex-twice input padding-sm margin-xs radius">
					<input type="text" v-model="bdbh" />
				</view>
			</view>
			<view class="flex  p-xs margin-bottom-sm mb-sm">
				<view class="flex-sub reg_font padding-sm margin-xs radius">
					<text>联系地址</text>
				</view>
				<view class="flex-twice input padding-sm margin-xs radius">
					<input type="text" v-model="bdbh" />
				</view>
			</view>
			<view class="flex  p-xs margin-bottom-sm mb-sm">
				<view class="flex-sub reg_no padding-sm margin-xs radius">
					<button @click="onRegister"></button>
				</view>
				<view class="flex-sub cancel_no padding-sm margin-xs radius">
					<button @click="onBack"></button>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				cjyxm: '1',
				bdbh: '2',
				lxdh: '3',
				lxdz: '4',
				xb: '男',
			}
		},
		methods: {
			selectSex(evt) {
				console.log(evt.target.value);
			},
			async onRegister() {
				//验证数据
				if (this.cjyxm == "" || this.bdbh == "" || this.lxdh == "" || this.lxdz == "") {
					uni.showToast({
						title: "数据格式有误",
						icon: "none"
					}); 
				} else {
					//TODO 正则校验数据
					let userData = await getApp().fileReader(getApp().globalData.cjyData);
		
					let userDataArray = eval(userData);
					let currentUser = {
						id: '',
						createTime: '',
						updateTime: '',
						cjyxm: this.cjyxm,
						bdbh: this.bdbh,
						xb: this.xb,
						lxdh: this.lxdh,
						lxdz: this.lxdz,
						bz: "1"
					}
					userDataArray.push(currentUser);
				
					getApp().fileWriter(getApp().globalData.cjyData, JSON.stringify(userDataArray));
				}

			},
			onBack() {
				// uni.navigateBack({
				// 	delta: 1
				// });
				uni.navigateTo({
					url:"../index/index"  
				})
			}
		}
	}
</script>

<style>
	
	.reg_font{
		color: white;
	}
	
	.input{
		background-image: url(../../static/input/input.png);
	}
	
	.reg_bg{
		background: url(../../static/reg/reg.png);
		background-repeat: no-repeat;
		margin-top: 20%;
		padding-top: 180rpx;
	}
	
	.reg_no{
		background-image: url(../../static/reg/no_reg.png);
		background-repeat: no-repeat;
		width: 228px;
		height: 82px;
	}
	
	.cancel_no{
		background-image: url(../../static/reg/no_cancel.png);
		background-repeat: no-repeat;
		width: 228px;
		height: 82px;
	}
</style>
