<script>
	export default {
		globalData: {
			serverUrl: "",
			cjyData: 'cjyData.json'
		},
		onLaunch: function() {
			console.log('App Launch')
		},
		onShow: function() {
			console.log('App Show')
		},
		onHide: function() {
			console.log('App Hide')
		},
		methods: {
			//读取文件
			fileReader(fileName) {
				return new Promise((resolve) => {
					let realFileName = "_doc/" + fileName;
					uni.showLoading({
						title: "读取中",
					});
					let result;
					plus.io.resolveLocalFileSystemURL(realFileName, function(entry) {
						///读取文件  
						entry.file(function(file) {
							var fileReader = new plus.io.FileReader();
							fileReader.readAsText(file, 'utf-8');
							fileReader.onloadend = function(evt) {
								uni.hideLoading();
								// console.log(evt.target.result);
								// result = evt.target.result; 
								resolve(evt.target.result);
							}
						});
					});

				});


				return result;
			},
			//写入文件
			fileWriter: function(fileName, fileData) {
				plus.io.requestFileSystem(plus.io.PRIVATE_DOC, function(success) {
					success.root.getFile(fileName, { //查找gongyongchao这个文件，有就打开，没有就创建一个 
						create: true
					}, function(fileEntry) {
						var Path = fileEntry.toURL(); //获取当前文件的路径
						console.log(Path, '当前路径');
						fileEntry.createWriter(function(data) { //对文件进行写入操作
							data.write(fileData);
							console.log('写入成功')
						}, function(e) {
							console.log('写入失败')
						})
						console.log('创建了一个新的文件')
					})
					console.log('我已经进入了系统目录下')
				}, function(e) {
					console.log('我是调用失败的回调') //失败的回调
				});

			}
		}
	}
</script>

<style>
	/*每个页面公共css */
	@import "colorui/main.css";
	@import "colorui/icon.css";
	@import "colorui/app.css";
</style>
