<template>
	<view class="container container25504">
		<u-navbar titleWidth="500" :isFixed="true" :isSlotTitle="true" title="兑换" backIconColor="#fff" titleColor="inherit" bgColor="red" :backTextStyle="{ color: 'inherit' }" :isHome="false" :isBack="true">
			<template v-slot:title> 兑换 </template>
		</u-navbar>
		<view class="flex flex-wrap diygw-col-24 flex-direction-column flex-clz">
			<view class="diygw-col-24 text1-clz"> {{share.title}} </view>
			<view class="flex flex-wrap diygw-col-24 flex1-clz">
				<view class="diygw-col-8 text-clz"> 作者：{{share.author}} </view>
				<view class="diygw-col-8 text4-clz"> 发布人：{{nickname}} </view>
				<view class="diygw-col-8 text3-clz"> 下载次数：{{share.buyCount}} </view>
			</view>
			<view class="flex flex-wrap diygw-col-24 justify-center flex2-clz">
				<image :src="share.cover" class="response diygw-col-24 image-clz" mode="widthFix"></image>
				<view class="diygw-col-24 text5-clz"> {{share.summary}} </view>
			</view>
		</view>
		<view class="flex flex-wrap diygw-col-24 justify-end flex3-clz">
			<view class="diygw-col-8 text6-clz"> 积分：{{share.price}} </view>
			<view class="flex diygw-col-8 button-clz">
				<button @tap="exchange" data-type="tip" data-tip="兑换成功！" class="diygw-btn red radius-xs flex-sub margin-xs button-button-clz">兑换</button>
			</view>
		</view>
		<view class="clearfix"></view>
	</view>
</template>

<script>
	import {
		request
	} from '@/utils/request';
	
	import {
		SHARE_URL,
		EXCHANGE_URL
	} from '@/utils/api';
	export default {
		data() {
			return {
				share: {},
				nickname: '',
				avatarUrl: '',
				id: ''
			};
		},
		onShow() {
			this.setCurrentPage(this);
			this.getShare();
		},
		onLoad(option) {
			this.id = option.id;
			console.log(this.id);
			// this.getShare();
		},
		methods: {
			async getShare() {
				let id = this.id;
				uni.showLoading({
					title: '加载中'
				});
				let res = await request(SHARE_URL + `/${id}`,'GET');
				setTimeout(() => {
					uni.hideLoading();
				},1000);
				console.log(res.data);
				this.share = res.data.share;
				this.nickname = res.data.nickname;
				this.avatarUrl = res.data.avatarUrl;
			},
			async exchange() {
				await request(EXCHANGE_URL, 'POST', {
					userId: uni.getStorageSync("user").id,
					shareId: this.share.id
				}).then(res => {
					uni.showModal({
						title: '兑换成功',
						content: '点击确定去查看，取消则继续兑换',
						success: res => {
							if(res.confirm) {
								uni.redirectTo({
									url: `/pages/detail/detail?id=${this.id}`
								});
							} else if (res.cancel) {
								uni.switchTab({
									url: "/pages/index/index"
								});
							}
						}
					});
				});
			}
		}
	};
</script>

<style lang="scss" scoped>
	.flex-clz {
		margin-left: 10rpx;
		width: calc(100% - 10rpx - 10rpx) !important;
		margin-top: 10rpx;
		margin-bottom: 10rpx;
		margin-right: 10rpx;
	}
	.text1-clz {
		font-weight: bold;
		font-size: 36rpx !important;
	}
	.flex1-clz {
		padding-top: 10rpx;
		padding-left: 10rpx;
		padding-bottom: 10rpx;
		padding-right: 10rpx;
	}
	.text-clz {
		font-weight: bold;
	}
	.text4-clz {
		font-weight: bold;
	}
	.text3-clz {
		font-weight: bold;
	}
	.flex2-clz {
		margin-left: 10rpx;
		width: calc(100% - 10rpx - 10rpx) !important;
		margin-top: 10rpx;
		margin-bottom: 10rpx;
		text-align: center;
		margin-right: 10rpx;
	}
	.image-clz {
		flex-shrink: 0;
		border-bottom-left-radius: 12rpx;
		overflow: hidden;
		width: 360rpx !important;
		border-top-left-radius: 12rpx;
		border-top-right-radius: 12rpx;
		border-bottom-right-radius: 12rpx;
		height: 360rpx !important;
		text-align: center;
	}
	.text5-clz {
		margin-left: 10rpx;
		width: calc(100% - 10rpx - 10rpx) !important;
		font-size: 28rpx !important;
		margin-top: 10rpx;
		margin-bottom: 10rpx;
		text-align: left;
		margin-right: 10rpx;
	}
	.flex3-clz {
		text-align: center;
	}
	.text6-clz {
		margin-left: 10rpx;
		padding-top: 10rpx;
		padding-left: 10rpx;
		width: calc(33.3333333333% - 10rpx - 10rpx) !important;
		font-size: 32rpx !important;
		padding-bottom: 10rpx;
		margin-top: 10rpx;
		margin-bottom: 10rpx;
		text-align: center;
		margin-right: 10rpx;
		padding-right: 10rpx;
	}
	.button-clz {
		transition: color 300ms linear, color 300ms linear;
	}
	.button-clz:active {
		font-size: 24rpx !important;
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
