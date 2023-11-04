<template>
	<view class="content">
		<view class="logo">
			<u-image src="/static/wxlogo.png" shape="circle" width="200rpx" height="200rpx"></u-image>
		</view>
		<u-transition :show="true" mode="slide-right">
			<view class="desc">An intelligent assistant powered by Chat-GPT</view>
		</u-transition>
		<view class="confirm">
			<u-input class="admin_input" v-model="server_host" type="text" placeholder="Enter server domain/address" />
			<view class="success" v-show="success">Settings saved successfully</view>
			<u-button class="admin_btn" shape="circle" @click="change">Confirm</u-button>
			<u-button class="reset_btn" shape="circle" color="#26B3A0" :plain="true" @click="reset">Reset</u-button>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				success: false,
				server_host: ""
			}
		},
		onLoad() {
			this.server_host = uni.getStorageSync('server_host');
			this.success = false;
		},
		methods: {
			reset() {
				this.server_host = "";
			},
			change() {
				// Set the server address without validation
				uni.setStorageSync("server_host", this.server_host);
				this.success = true;
				setTimeout(function() {
					uni.navigateBack();
				}, 1000);
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
