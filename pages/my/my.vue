<template>
	<view class="container container25504">
		
		
		
		
		<view class="flex flex-wrap diygw-col-24 flex-direction-column" v-if="!userInfo">
			<view class="flex diygw-col-24 justify-center avatar-clz">
				<view class="diygw-avatar radius bg-none">
					<image mode="aspectFit" class="diygw-avatar-img radius" src="/static/unlogin.png"></image>
				</view>
			</view>
			<view class="flex flex-wrap diygw-col-24 justify-center">
				<view class="diygw-col-5 text-clz"> 登录，享受技术之旅 </view>
				<view class="flex flex-wrap diygw-col-24 justify-center flex2-clz">
					<view class="flex diygw-col-8">
						<button class="diygw-btn red radius-xs flex-sub margin-xs button-button-clz"
						  @tap="navigateTo" data-type="page" data-url="/pages/login/login">登录</button>
					</view>
				</view>
			</view>
		</view>
		
		
		
		<view class="flex flex-wrap diygw-col-24 flex-direction-column" v-else>
			<view class="flex diygw-col-24 justify-center avatar-clz">
				<view class="diygw-avatar radius bg-none">
					<image mode="aspectFit" class="diygw-avatar-img radius" :src="userInfo.avatarUrl"></image>
				</view>
			</view>
			<view class="flex flex-wrap diygw-col-24 justify-center">
				<view class="diygw-col-5 text-clz"> {{userInfo.nickname}} </view>
				<view class="diygw-col-24 text1-clz"> 积分：{{userInfo.bonus}} </view>
				<view class="flex flex-wrap diygw-col-24 justify-center flex2-clz">
					<view class="flex diygw-col-8">
						<button class="diygw-btn blue radius-xs flex-sub margin-xs button1-button-clz">签到</button>
					</view>
					<view class="flex diygw-col-8">
						<button @tap="logout" 
						class="diygw-btn blue radius-xs flex-sub margin-xs button-button-clz">退出</button>
					</view>
				</view>
			</view>
			<view v-if="userInfo">
				<view class="flex flex-wrap diygw-col-24" @tap="navigateTo" data-type="page" data-url="/pages/exchange/myexchange">
					<view class="diygw-col-22 text2-clz"> 我的兑换 </view>
					<image src="/static/yjt.png" class="response diygw-col-2 image-clz" mode="widthFix"></image>
				</view>
				<view class="flex diygw-col-24">
					<view class="diygw-pzx" style="border-bottom: 1px solid #eee"></view>
				</view>
				<view class="flex flex-wrap diygw-col-24" @tap="navigateTo" data-type="page" data-url="/pages/point/pointdetail">
					<view class="diygw-col-22 text5-clz"> 积分明细 </view>
					<image src="/static/yjt.png" class="response diygw-col-2 image3-clz" mode="widthFix"></image>
				</view>
				<view class="flex diygw-col-24">
					<view class="diygw-pzx" style="border-bottom: 1px solid #eee"></view>
				</view>
				<view class="flex flex-wrap diygw-col-24" @tap="navigateTo" data-type="page" data-url="/pages/submission/mysubmission">
					<view class="diygw-col-22 text4-clz"> 我的投稿 </view>
					<image src="/static/yjt.png" class="response diygw-col-2 image2-clz" mode="widthFix"></image>
				</view>
				<view class="flex diygw-col-24">
					<view class="diygw-pzx" style="border-bottom: 1px solid #eee"></view>
				</view>
				<view class="flex flex-wrap diygw-col-24"  v-if="userInfo.roles === 'admin'" @tap="navigateTo" data-type="page" data-url="/pages/audit/waitAudit">
					<view class="diygw-col-22 text3-clz"> 审核投稿 </view>
					<image src="/static/yjt.png" class="response diygw-col-2 image1-clz" mode="widthFix"></image>
				</view>
			</view>
		</view>
		<view class="clearfix"></view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				//用户全局信息
				userInfo: null,
				//页面传参
				globalOption: {},
				//自定义全局变量
				globalData: {}
			};
		},
		onShow() {
			this.setCurrentPage(this);
			if(uni.getStorageSync('user')) {
				this.userInfo = uni.getStorageSync('user');
			}
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
		methods: {
			async init() {},
			logout() {
				this.userInfo = null;
				uni.clearStorageSync();
			}
		}
	};
</script>

<style lang="scss" scoped>
	.avatar-clz {
		margin-left: 60rpx;
		width: calc(100% - 60rpx - 60rpx) !important;
		margin-top: 60rpx;
		margin-bottom: 60rpx;
		margin-right: 60rpx;
	}
	.text-clz {
		margin-left: 10rpx;
		font-weight: bold;
		flex: 1;
		width: calc(20.8333333333% - 10rpx - 10rpx) !important;
		margin-top: 10rpx;
		margin-bottom: 10rpx;
		text-align: center;
		margin-right: 10rpx;
	}
	.text1-clz {
		margin-left: 10rpx;
		width: calc(100% - 10rpx - 10rpx) !important;
		margin-top: 10rpx;
		margin-bottom: 10rpx;
		text-align: center;
		margin-right: 10rpx;
	}
	.flex2-clz {
		margin-left: 10rpx;
		padding-top: 10rpx;
		padding-left: 10rpx;
		width: calc(100% - 10rpx - 10rpx) !important;
		padding-bottom: 10rpx;
		margin-top: 10rpx;
		margin-bottom: 10rpx;
		margin-right: 10rpx;
		padding-right: 10rpx;
	}
	.button1-button-clz {
		margin: 3px !important;
	}
	.button-button-clz {
		margin: 3px !important;
	}
	.text2-clz {
		margin-left: 10rpx;
		padding-top: 10rpx;
		padding-left: 10rpx;
		width: calc(91.6666666667% - 10rpx - 10rpx) !important;
		padding-bottom: 10rpx;
		margin-top: 10rpx;
		margin-bottom: 10rpx;
		margin-right: 10rpx;
		padding-right: 10rpx;
	}
	.image-clz {
		text-align: right;
	}
	.text5-clz {
		margin-left: 10rpx;
		padding-top: 10rpx;
		padding-left: 10rpx;
		width: calc(91.6666666667% - 10rpx - 10rpx) !important;
		padding-bottom: 10rpx;
		margin-top: 10rpx;
		margin-bottom: 10rpx;
		margin-right: 10rpx;
		padding-right: 10rpx;
	}
	.image3-clz {
		text-align: right;
	}
	.text4-clz {
		margin-left: 10rpx;
		padding-top: 10rpx;
		padding-left: 10rpx;
		width: calc(91.6666666667% - 10rpx - 10rpx) !important;
		padding-bottom: 10rpx;
		margin-top: 10rpx;
		margin-bottom: 10rpx;
		margin-right: 10rpx;
		padding-right: 10rpx;
	}
	.image2-clz {
		text-align: right;
	}
	.text3-clz {
		margin-left: 10rpx;
		padding-top: 10rpx;
		padding-left: 10rpx;
		width: calc(91.6666666667% - 10rpx - 10rpx) !important;
		padding-bottom: 10rpx;
		margin-top: 10rpx;
		margin-bottom: 10rpx;
		margin-right: 10rpx;
		padding-right: 10rpx;
	}
	.image1-clz {
		text-align: right;
	}
	.container25504 {
		padding-left: 0px;
		padding-right: 0px;
	}
	.container25504 {
	}
</style>
