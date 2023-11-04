<template>
	<view class="content">
		<view class="logo">
			<u-image src="/static/wxlogo.png" shape="circle" width="200rpx" height="200rpx"></u-image>
		</view>
		<u-transition :show="true" mode="slide-right">
			<view class="desc">An intelligent assistant powered by Chat-GPT</view>
		</u-transition>
		<view class="confirm">
			<u-input class="admin_input" v-model="ch_email" type="text" placeholder="Enter your ChatGPT account" />
			<u-input class="admin_input" v-model="ch_password" type="password" placeholder="Enter your ChatGPT password" />
			<view class="success" v-show="success">Login successful</view>
			<view class="wrong" v-show="wrong">Login failed</view>
			<u-button class="admin_btn" shape="circle" @click="login">Login to ChatGPT</u-button>
			<u-button class="reset_btn" shape="circle" color="#26B3A0" :plain="true" @click="reset">Reset</u-button>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				success: false,
				wrong: false,
				ch_email: "",
				ch_password: "",
				ch_token: "",
				server_host: ""
			}
		},
		onLoad() {
			this.ch_email = uni.getStorageSync('ch_email');
			this.ch_password = uni.getStorageSync('ch_password');
			this.server_host = uni.getStorageSync('server_host');
			this.success = false;
			this.wrong = false;
		},
		methods: {
			reset() {
				this.ch_email = "";
				this.ch_password = "";
				uni.clearStorageSync();
			},
			login() {
				const _this = this;
				uni.showLoading({
					title: "Logging in..."
				})
				uni.request({
					// Backend API address

					url: _this.server_host + "/login",
					method: "POST",
					data: {
						"login_data": {
							"username": _this.ch_email,
							"password": _this.ch_password
						}
					},
					success: function(res) {
						uni.hideLoading();
						if (res.data["code"] === "1") {
							_this.ch_token = res.data['token'];
							_this.success = true;
							uni.setStorageSync('ch_email', _this.ch_email);
							uni.setStorageSync('ch_password', _this.ch_password);
							setTimeout(() => {
								_this.success = false;
								_this.wrong = false;
								uni.navigateTo({
									url: "/pages/main/form/index?token=" + _this.ch_token
								}, 2)
							})
						} else {
							_this.wrong = true;
						}
					},
					fail: function(res) {
						uni.hideLoading();
						_this.wrong = true;
					}
				})
			}
		}
	}
</script>

<style lang="scss">
	.content {
		.logo {
			width: 100px;
			margin-top: 100px;
			margin-left: auto;
			margin-right: auto;
		}
		
		.desc {
			width: fit-content;
			margin-top: 10px;
			margin-left: auto;
			margin-right: auto;
			color: #696969;
		}

		.confirm {
			width: 600rpx;
			margin-top: 20px;
			margin-left: auto;
			margin-right: auto;

			.admin_input {
				height: 30px;
				margin-top: 10px;
				padding: 10px;
				border-radius: 20px;
			}

			.wrong {
				width: 100%;
				margin-top: 10px;
				text-align: center;
				color: red;
			}

			.success {
				width: 100%;
				margin-top: 10px;
				text-align: center;
				color: #26B3A0;
			}

			.admin_btn {
				width: 100%;
				height: 45px;
				margin-top: 10px;
				background-color: #26B3A0;
				color: #ffffff;
				font-size: 16px;
			}

			.reset_btn {
				margin-top: 10px;
				font-size: 16px;
			}
		}
	}
</style>
