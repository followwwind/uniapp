<template>
	<view class="padding bg">
		<view class="flex  p-xs margin-bottom-sm mb-sm">
			<view class="flex-sub bg-grey padding-sm margin-xs radius">文本采集</view>
			<view class="flex-twice bg-grey padding-sm margin-xs radius">
				<input type="text" v-model="wbcj" />
			</view>
		</view>
		<view class="flex  p-xs margin-bottom-sm mb-sm">
			<view class="flex-sub bg-grey padding-sm margin-xs radius">选择</view>
			<view class="flex-twice bg-grey padding-sm margin-xs radius">
				<picker @change="bindXzChange" :range="xz" mode="selector">
					<label class="">{{xz[indexXz]}}</label>
				</picker>
			</view>
		</view>
		<view class="flex  p-xs margin-bottom-sm mb-sm">
			<view class="flex-sub bg-grey padding-sm margin-xs radius">选择装备</view>
			<view class="flex-twice bg-grey padding-sm margin-xs radius">
				<picker @change="bindXzzbChange" :range="xzzb" mode="selector">
					<label class="">{{xzzb[indexXzzb]}}</label>
				</picker>
			</view>
		</view>
		<view class="flex  p-xs margin-bottom-sm mb-sm">
			<!-- 		<view class="flex-sub bg-grey padding-sm margin-xs radius">时间采集</view>
			<view class="flex-twice bg-grey padding-sm margin-xs radius">
				
				<button>111</button>
			</view> -->
			<view class="flex-twice bg-grey padding-sm margin-xs radius">时间采集</view>
			<view class="flex-treble bg-grey padding-sm margin-xs radius">
				<text>{{sjcj}}</text>
			</view>
			<view class="flex-sub bg-grey padding-sm margin-xs radius">
				<button @click="getTime()" class="cu-btn round sm" style="width: 100%;">采集</button>
			</view>
		</view>
		<view class="flex  p-xs margin-bottom-sm mb-sm">
			<view class="flex-twice bg-grey padding-sm margin-xs radius">拍照采集</view>
			<view class="flex-treble bg-grey padding-sm margin-xs radius">
				<text>{{pzcj}}</text>
			</view>
			<view class="flex-sub bg-grey padding-sm margin-xs radius">
				<button @click="onSelectPhoto()" class="cu-btn round sm" style="width: 100%;">采集</button>
			</view>
		</view>
		<view class="flex  p-xs margin-bottom-sm mb-sm">

			<view class="flex-twice bg-grey padding-sm margin-xs radius">录像采集</view>
			<view class="flex-treble bg-grey padding-sm margin-xs radius">
				<text>{{lxcj}}</text>
			</view>
			<view class="flex-sub bg-grey padding-sm margin-xs radius">
				<button @click="onSelectVideo()" class="cu-btn round sm" style="width: 100%;">采集</button>
			</view>
		</view>
		<view class="flex  p-xs margin-bottom-sm mb-sm">
			<view class="flex-twice bg-grey padding-sm margin-xs radius">录音采集</view>
			<view class="flex-treble bg-grey padding-sm margin-xs radius">
				<text>{{lycj}}</text>
			</view>
			<view class="flex-sub bg-grey padding-sm margin-xs radius">
				<button @tap="showModal" data-target="ChooseModal" class="cu-btn round sm"
					style="width: 100%;">采集</button>
			</view>
		</view>

		<view class="flex  p-xs margin-bottom-sm mb-sm">
			<view class="flex-twice bg-grey padding-sm margin-xs radius">秒表采集</view>
			<view class="flex-treble bg-grey padding-sm margin-xs radius">
				<text v-bind:"{{mbcj}}"></text> 
			</view>
			<view class="flex-sub bg-grey padding-sm margin-xs radius">
				<button @click="startTimer()" class="cu-btn round sm" style="width: 100%;">计时</button>
			</view>
		</view>

		<view class="flex  p-xs margin-bottom-sm mb-sm">
			<view class="flex-sub bg-grey padding-sm margin-xs radius">
				<button @click="onSubmit">上传结果</button>
			</view>
			<view class="flex-sub bg-grey padding-sm margin-xs radius">
				<button @click="onBack">返回模型选择</button>
			</view>
		</view>

		<view class="cu-modal bottom-modal" :class="modalName=='ChooseModal'?'show':''" @tap="hideModal">
			<view class="cu-dialog" @tap.stop="">
				<sound-recording v-on:confirm="confirm" v-on:cancel="cancel"></sound-recording>
			</view>
		</view>
	</view>
</template>

<script>
	import soundRecording from '@/components/sound-recording/sound-recording.vue';
	import luchAudio from '@/components/luch-audio/luch-audio.vue';
	export default {
		components: {
			soundRecording
		},
		data() {
			return {
				wbcj: '',
				xz: ['--请选择--', '111', '222'],
				indexXz: 0,
				xzzb: ['--请选择--', '111', '222'],
				indexXzzb: 0,
				sjcj: '请点击采集',
				pzcj: '请点击拍照',
				lxcj: '请点击录像',
				lycj: '请点击录音',
				mbcj: '请点击计时',
				recorderManager: {},
				innerAudioContext: {},
				voicePath: '',
				modalName: null,
			}
		},
		methods: {
			onLoad() {

			},
			onSubmit: function() {

			},
			onBack() {
				uni.navigateBack({
					delta: 1
				});
			},
			bindXzChange(e) {
				//console.log('picker发送选择改变，携带值为', e.target.value)   用于输出改变索引值
				this.indexXz = e.target.value //将数组改变索引赋给定义的index变量
				// this.jg = this.array[this.index]

			},
			bindXzzbChange(e) {
				this.indexXzzb = e.target.value //将数组改变索引赋给定义的index变量
			},
			getTime() {
				var date = new Date(),
					year = date.getFullYear(),
					month = date.getMonth() + 1,
					day = date.getDate(),
					hour = date.getHours() < 10 ? "0" + date.getHours() : date.getHours(),
					minute = date.getMinutes() < 10 ? "0" + date.getMinutes() : date.getMinutes(),
					second = date.getSeconds() < 10 ? "0" + date.getSeconds() : date.getSeconds();
				month >= 1 && month <= 9 ? (month = "0" + month) : "";
				day >= 0 && day <= 9 ? (day = "0" + day) : "";
				var timer = year + '-' + month + '-' + day + ' ' + hour + ':' + minute + ':' + second;
				this.sjcj = timer;
			},
			onSelectPhoto() {
				uni.chooseImage({
					count: 1, //默认9
					sizeType: ['compressed'], //可以指定是原图还是压缩图，默认二者都有
					sourceType: ['camera'], //从相册选择
					success: function(res) {
						console.log(JSON.stringify(res.tempFilePaths));
					}
				});
			},
			onSelectVideo() {
				var self = this;
				uni.chooseVideo({
					count: 1,
					sourceType: ['camera'],
					success: function(res) {
						self.src = res.tempFilePath;
						console.log(JSON.stringify(res.tempFilePath));
					}
				});
			},
			startRecord() {
				//跳转
				uni.navigateTo({
					url: "../recordAudio/recordAudio",
				});
			},
			//秒表计时
			startTimer() {
				var hour, minute, second; /*时 分 秒*/
				hour = minute = second = 0; //初始化
				var millisecond = 0; //毫秒
				this.mbcj = setInterval(() => {
					millisecond = millisecond + 50;
					// console.log("---millisecond----"+millisecond);
					if (millisecond >= 1000) {
						millisecond = 0;
						second = second + 1;
					}
					if (second >= 60) {
						second = 0;
						minute = minute + 1;
					}

					if (minute >= 60) {
						minute = 0;
						hour = hour + 1;
					}
					// console.log("-------"+hour+'时'+minute+'分'+second+'秒');
					this.nums = hour + ':' + minute + ':' + second + ':';
				}, 50);
			},
			showModal(e) {
				this.modalName = e.currentTarget.dataset.target
			},
			hideModal(e) {
				this.modalName = null
			},
			confirm(e) {
				console.log(e);
				this.hideModal();
			},
			cancel() {
				console.log("cancel");
				this.hideModal();
			}
		}
	}
</script>

<style>

</style>
