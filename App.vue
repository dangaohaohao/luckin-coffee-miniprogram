<script>
	export default {
		methods: {
			// 处理是否登录
			// 从本地获取 isLogin,判断是否登录
			// 如果 isLogin 为 true,则不做什么
			// 如果 isLogin 为 false, 则调用云函数，登录
			handleLogin() {
				try {
				    const isLogin = uni.getStorageSync('isLogin');
					
				    if (isLogin) {
				        console.log('已经登录', isLogin);
				    }else {
						wx.cloud.callFunction({
							name: 'get-login'
						})
						.then(res => {
							if(res.errMsg.endsWith('ok')) {
								uni.setStorage({
								    key: 'isLogin',
								    data: true,
								    success: function () {
								        console.log('保存用户登录态成功');
								    },
									fail: function () {
										console.log('保存用户登录态失败');
									}
								});
							}
						})
						.catch(err => {
							console.log(err);
							console.log('调用云函数出错');
						})
					}
					
				} catch (e) {
				    console.log('获取isLogin出错');
					console.log(e);
				}
			}
		},
		onLaunch: async function() {
			console.log('App Launch')
			await wx.cloud.init({
			  env: 'yuzhenliu-dev-r3cgh'
			})
			this.handleLogin();
		},
		onShow: function() {
			console.log('App Show')
		},
		onHide: function() {
			console.log('App Hide')
		}
	}
</script>

<style>
	/*每个页面公共css */
	page {
		width: 100%;
		height: 100%;
	}
</style>
