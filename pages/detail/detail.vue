<template>
	<view class="container container25504">
		<u-navbar titleWidth="500" :isFixed="true" :isSlotTitle="true" title="详情" backIconColor="#fff" titleColor="inherit" bgColor="red" :backTextStyle="{ color: 'inherit' }" :isHome="false" :isBack="true">
			<template v-slot:title> 资源详情 </template>
		</u-navbar>
		<view class="flex flex-wrap diygw-col-24 flex-direction-column flex-clz">
			<view class="diygw-col-24 text1-clz"> {{share.title}} </view>
			<view class="flex flex-wrap diygw-col-24 flex1-clz">
				<view class="diygw-col-7 text-clz"> 作者：{{share.author}} </view>
				<view class="diygw-col-7 text4-clz"> 发布人：{{nickname}} </view>
				<view class="diygw-col-5 text3-clz"> 积分：{{share.price}} </view>
				<view v-if="share.isOriginal" class="diygw-col-5 text2-clz"> 来源：原创 </view>
				<view v-else class="diygw-col-5 text2-clz"> 来源：转载 </view>
				<!-- 待处理，显示：原创or转载 -->
			</view>
			<view class="flex flex-wrap diygw-col-24 justify-center flex2-clz">
				<image :src="share.cover" class="response diygw-col-24 image-clz" mode="widthFix"></image>
				<view class="diygw-col-24 text5-clz"> {{share.summary}} </view>
				<view class="diygw-col-24 text6-clz"> {{share.downloadUrl}} </view>
			</view>
		</view>
		<view class="flex diygw-col-24">
			<view class="diygw-pzx" style="border-bottom: 1px solid #eee"></view>
		</view>
		<view class="flex diygw-col-24">
			<button @tap="navigateTo" data-type="copy" :data-copy="share.downloadUrl" data-tip="已成功复制！" class="diygw-btn grey radius-xs flex-sub margin-xs button-button-clz">复制下载链接</button>
			<!-- 复制，待解决 -->
		</view>
		<view class="clearfix"></view>
	</view>
</template>

<script>
	import {
		request
	} from '@/utils/request';
	
	import {
		SHARE_URL
	} from '@/utils/api';
	
	export default {
		data() {
			return {
				share: null,
				nickname: '',
				avatarUrl: '',
				id: ''
			};
		},
		onShow() {},
		onLoad(option) {
			this.id = option.id;
			console.log(this.id);
			this.getShare();
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
				},500);
				console.log(res.data);
				this.share = res.data.share;
				this.nickname = res.data.nickname;
				this.avatarUrl = res.data.avatarUrl;
			},
			paste() {
				uni.setClipboardData({
					data: this.share.downloadUrl,
					success: function() {
						uni.showToast({
							title: '下载地址已复制'
						});
					}
				});
			}
		}
	};
</script>

<style lang="scss" scoped>
	.text4-clz {
		font-weight: bold;
	}
	.text3-clz {
		font-weight: bold;
	}
	.text2-clz {
		font-weight: bold;
	}
	.text-clz {
		font-weight: bold;
	}
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
	.text6-clz {
		margin-left: 10rpx;
		width: calc(100% - 10rpx - 10rpx) !important;
		font-size: 28rpx !important;
		margin-top: 10rpx;
		margin-bottom: 10rpx;
		text-align: left;
		margin-right: 10rpx;
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