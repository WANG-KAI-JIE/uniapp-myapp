<template>
	<view class="container container25504">
		<view class="flex flex-wrap diygw-col-24 justify-center flex-clz">
			<u-form :model="form" :rules="formRules" :errorType="['message','toast']" ref="formRef" class="flex diygw-form diygw-col-24"></u-form>
			
			<view class="flex diygw-col-10 justify-center avatar-clz">
				<view class="diygw-avatar avatar-avatar xl margin-md bg-none">
					<image mode="aspectFit" class="diygw-avatar-img" src="/static/loginlogo.png"></image>
				</view>
			</view>
			
			<view class="flex flex-wrap diygw-col-24 justify-center">
				<u-form-item class="diygw-col-24" label="手机号" prop="input">
					<u-input :focus="inputFocus" class="" placeholder="请输入手机号" v-model="phone" type="text"></u-input>
				</u-form-item>
				<u-form-item class="diygw-col-24" label="密码" prop="input1">
					<u-input :focus="input1Focus" class="" placeholder="请输入密码" v-model="password" type="text"></u-input>
				</u-form-item>
				<view class="flex diygw-col-7 justify-center button-clz">
					<button @tap="login" :disabled="disabled" 
					class="diygw-btn red radius-xs flex-sub margin-xs button-button-clz">登录</button>
				</view>
			</view>
		</view>
		<view class="clearfix"></view>
	</view>
</template>

<script>
	import
	{
		request
	} from '@/utils/request';
	
	import {
		LOGIN_URL
	} from '@/utils/api';
	
	export default {
		data() {
			return {
				//用户全局信息
				userInfo: {},
				//页面传参
				globalOption: {},
				//自定义全局变量
				globalData: {},
				inputFocus: false,
				phone: '18120190176',
				input1Focus: false,
				password: '123456'
			};
		},
		computed: {
			disabled() {
				if((this.phone === '' || this.password === '')) {
					return true
				}
				return false;
			}
		},
		onShow() {
			this.setCurrentPage(this);
		},
		onLoad(option) {
			this.setCurrentPage(this);
			if (option) {
				this.setData({
					globalOption: this.getOption(option)
				});
			}

			this.init();
		},
		onReady() {
			this.$refs.formRef?.setRules(this.formRules);
		},
		methods: {
			async init() {
				await this.initResetForm();
			},
			initResetForm() {
				this.initform = JSON.stringify(this.form);
			},
			resetForm() {
				this.form = JSON.parse(this.initform);
			},
			async login() {
				const loginDTO = {
					phone: this.phone,
					password: this.password
				}
				const res = await request(LOGIN_URL,'POST',loginDTO);
				console.log(res)
				if(res.success == true) {
					uni.showToast({
						title: '登录成功'
					});
					console.log(JSON.stringify(res.data.user) + '&&&&&&&&')
					uni.setStorageSync('user',res.data.user);
					uni.setStorageSync('token',res.data.token);
					uni.switchTab({
						url: "/pages/my/my"
					})
				}
			}
		}
	};
</script>

<style lang="scss" scoped>
	.flex-clz {
		margin-left: 10rpx;
		padding-top: 10rpx;
		padding-left: 10rpx;
		width: calc(100% - 10rpx - 10rpx) !important;
		padding-bottom: 10rpx;
		margin-top: 10rpx;
		margin-bottom: 10rpx;
		text-align: center;
		margin-right: 10rpx;
		padding-right: 10rpx;
	}
	.avatar-clz {
		flex: 1;
		text-align: center;
	}
	.avatar-avatar {
		width: 100px;
		height: 100px;
	}
	.button-clz {
		text-align: center;
	}
	.button-button-clz {
		margin: 3px !important;
	}
	.container25504 {
		padding-left: 0px;
		padding-right: 0px;
	}
	.container25504 {
	}
</style>
