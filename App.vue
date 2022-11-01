<script>
	import Vue from 'vue'
	export default {
		onLaunch: async function(options) {
			let that = this;
			// #ifdef APP-PLUS
			plus.runtime.getProperty(plus.runtime.appid, function(widgetInfo) {
				// 获取 app的version
				that.appversion = widgetInfo.version
				try {
					uni.setStorageSync('appversion', that.appversion);
				} catch (e) {}
			})
			plus.navigator.closeSplashscreen()
			that.checkAppUpdate()
				.finally(() => {
					uni.hideLoading();
				})
			// token标志来判断
			let token = uni.getStorageSync('userToken');
			console.log(!token)
			if (!token) {
				//不存在则跳转至登录页
				uni.reLaunch({
					url: "/pages/login/index",
					success: () => {
						plus.navigator.closeSplashscreen();
					}
				})
			} else {
				//存在则关闭启动页进入首页
				plus.navigator.closeSplashscreen();
			}
			// #endif
			let platform = uni.getSystemInfoSync().platform;
			if (platform == 'android') {
				that.type = 0
			} else if (platform == 'ios') {
				that.type = 1
			}
			console.log(platform, 'platform')
		},
		data() {
			return {
				type: null,
				newArr: {},
				appversion: '',
				timer: null
			}
		},
		methods: {
			//检查app更新
			checkAppUpdate() {
				let that = this;
				return new Promise((resolve, reject) => {
					plus.runtime.getProperty(plus.runtime.appid, function(widgetInfo) {
						// that.appversion = widgetInfo.version
						// 存缓存 版本号
						console.log("appversion:" + that.appversion);
						that.$u.api.getVersion({
							osType: that.type
						}).then(res => {
							if (res.code == 0 && res.items != null) {
								that.newArr = res.items
								that.checkOK()
							}
							console.log(res, that.newArr, 'res')
						})

					})
				})

			},
			//确认更新
			checkOK() {
				let that = this;
				let newVersion = that.newArr.versionNo;
				// let newVersion = '2.0';
				console.log(newVersion, that.appversion < newVersion)
				if (that.appversion < newVersion) {
					// 需要更新
					// this.needUpdate = true;
					console.log(that.appversion, 'that.appversion')
					uni.showModal({
						title: '版本更新提示',
						content: 'APP发现新版本，请进行更新',
						confirmText: '更新',
						confirmColor: '#cb0725',
						success: res => {
							console.log(res.confirm)
							if (res.confirm === true) {
								that.checkWgtUpdate();
							} else {
								uni.hideLoading()
								clearTimeout(that.timer)
								that.timer = setTimeout(() => {
									uni.switchTab({
										url: '/pages/index/default/default'
									})
								}, 2000)
							}
						}
					})
				}
			},
			//更新
			checkWgtUpdate() {
				let that = this;
				console.log(that.newArr)
				let downloadUrl = [];
				if (this.newArr.link1 != null) downloadUrl.push(this.newArr.link1)
				if (this.newArr.link2 != null) downloadUrl.push(this.newArr.link2)
				if (this.newArr.link3 != null) downloadUrl.push(this.newArr.link3)
				let index = Math.floor(Math.random() * downloadUrl.length)
				console.log(downloadUrl, downloadUrl[index], index, 'downloadUrl[index]')
				plus.nativeUI.showWaiting("更新中...");
				let downloadTask = uni.downloadFile({ //执行下载
					url: downloadUrl[index],
					success: downloadResult => {
						console.log('下载', downloadResult)
						uni.hideLoading()
						console.log(downloadResult.statusCode)
						if (downloadResult.statusCode === 200) {
							plus.runtime.install(downloadResult.tempFilePath, {
									force: true
								},
								function() {
									console.log("更新成功")
									plus.nativeUI.toast("更新成功");
									plus.runtime.restart();
								},
								function(e) {
									console.log("更新失败")
									plus.nativeUI.closeWaiting();
									utils.showToast('更新失败');
								}

							)
						}
						plus.nativeUI.closeWaiting();
					},
					fail: error => {
						console.log(error)
					}
				})
				//控制台打印下载包进度
				downloadTask.onProgressUpdate(res => {
					console.log(res.progress)
				})
				// downloadTask.start();
			},
		},
		// 		onLaunch() {
		// 			// #ifdef MP-WEIXIN
		// 			this.autoUpdate();
		// 			// #endif

		// 			// #ifdef APP-PLUS || APP-PLUS-NVUE
		// 			this.checkVersion()
		// 			// #endif

		// 			// 获取店铺配置信息  全局只请求一次
		// 			this.$u.api.shopConfigV2().then(res => {
		// 				this.$store.commit('config', res.data)
		// 				// #ifdef H5
		// 				//百度统计
		// 				if (res.data.statistics) {
		// 					var script = document.createElement("script");
		// 					script.innerHTML = res.data.statistics;
		// 					document.getElementsByTagName("body")[0].appendChild(script);
		// 				}
		// 				// #endif
		// 			})
		// 			//获取三级联动城市信息
		// 			this.$u.api.getAreaList().then(res => {
		// 				if (res.status) {
		// 					this.$db.set('areaList', res.data)
		// 				}
		// 			});

		// 		},
		// 		onShow: function(obj) {
		// 			// #ifdef MP-WEIXIN
		// 			this.$store.commit('scene', obj.scene)
		// 			console.log(obj);
		// 			// #endif
		// 			//console.log('App Show')
		// 			// 未登录
		// 			if (!uni.getStorageSync('userToken')) {
		// 				uni.reLaunch({
		// 					url: '/pages/login/index'
		// 				})
		// 			}
		// 		},
		// 		onHide: function() {
		// 			//console.log('App Hide')
		// 		},
		// 		methods: {
		// 			// #ifdef MP-WEIXIN
		// 			//微信小程序检测更新措施，方式更新功能后，要等待24小时内才刷新的问题。
		// 			autoUpdate() {
		// 				var self = this
		// 				// 获取小程序更新机制兼容
		// 				if (wx.canIUse('getUpdateManager')) {
		// 					//console.log("进入小程序自动更新检测");
		// 					const updateManager = wx.getUpdateManager()
		// 					//1. 检查小程序是否有新版本发布
		// 					updateManager.onCheckForUpdate(function(res) {
		// 						//console.log("进入小程序检测是否需要自动更新");
		// 						//console.log(res);
		// 						// 请求完新版本信息的回调
		// 						if (res.hasUpdate) {
		// 							//检测到新版本，需要更新，给出提示
		// 							wx.showModal({
		// 								title: '更新提示',
		// 								content: '检测到新版本，是否下载新版本并重启小程序？',
		// 								success: function(res) {
		// 									if (res.confirm) {
		// 										//2. 用户确定下载更新小程序，小程序下载及更新静默进行
		// 										self.downLoadAndUpdate(updateManager)
		// 									} else if (res.cancel) {
		// 										//用户点击取消按钮，需要强制更新，二次弹窗
		// 										wx.showModal({
		// 											title: '温馨提示~',
		// 											content: '本次版本更新涉及到新的功能添加，旧版本无法正常访问的哦~',
		// 											showCancel: false,
		// 											confirmText: "确定更新",
		// 											success: function(res) {
		// 												if (res.confirm) {
		// 													//下载新版本，并重新应用
		// 													self.downLoadAndUpdate(updateManager)
		// 												}
		// 											}
		// 										})
		// 									}
		// 								}
		// 							})
		// 						}
		// 					})
		// 				} else {
		// 					// 如果希望用户在最新版本的客户端上体验您的小程序，可以这样子提示
		// 					wx.showModal({
		// 						title: '提示',
		// 						content: '当前微信版本过低，无法使用该功能，请升级到最新微信版本后重试。'
		// 					})
		// 				}
		// 			},
		// 			downLoadAndUpdate(updateManager) {
		// 				var self = this
		// 				wx.showLoading();
		// 				//静默下载更新小程序新版本
		// 				updateManager.onUpdateReady(function() {
		// 					wx.hideLoading()
		// 					//新的版本已经下载好，调用 applyUpdate 应用新版本并重启
		// 					updateManager.applyUpdate()
		// 				})
		// 				updateManager.onUpdateFailed(function() {
		// 					// 新的版本下载失败
		// 					wx.showModal({
		// 						title: '已经有新版本了哟~',
		// 						content: '新版本已经上线啦~，请您删除当前小程序，重新搜索打开哟~',
		// 					})
		// 				})
		// 			},
		// 			// #endif
		// 			// #ifdef APP-PLUS || APP-PLUS-NVUE
		// 			// app更新检测
		// 			checkVersion() {
		// 				// 获取应用版本号
		// 				let version = plus.runtime.version;
		// 				//检测当前平台，如果是安卓则启动安卓更新
		// 				uni.getSystemInfo({
		// 					success: res => {
		// 						this.updateHandler(res.platform, version);
		// 					}
		// 				})
		// 			},
		// 			// 更新操作
		// 			updateHandler(platform, version) {
		// 				let data = {
		// 					platform: platform,
		// 					version: version
		// 				}
		// 				let _this = this;
		// 				this.$u.api.getAppVersion(data).then(res => {
		// 					if (res.status && res.data[0].version) {
		// 						const info = res.data[0];
		// 						if (info.version !== '' && info.version > version) {
		// 							uni.showModal({
		// 								//提醒用户更新
		// 								title: '更新提示',
		// 								content: info.note,
		// 								success: res => {
		// 									if (res.confirm) {
		// 										plus.runtime.openURL(info.download_url)
		// 									}
		// 								}
		// 							})
		// 						}
		// 					}
		// 				});
		// 			}
		// 			// #endif
		// 		}
	}
</script>
<style lang="scss">
	/*加载UViewUI*/
	@import "uview-ui/index.scss";
	/*公共css */
	@import 'static/style/coreCommon.scss';
</style>
