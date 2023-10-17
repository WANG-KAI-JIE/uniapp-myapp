<template>
	<view class="container container25504">
		<u-navbar titleWidth="500" :isFixed="true" :isSlotTitle="true" title="待审核列表" backIconColor="#fff" titleColor="inherit" bgColor="red" :backTextStyle="{ color: 'inherit' }" :isHome="false" :isBack="true">
			<template v-slot:title> 待审核列表 </template>
		</u-navbar>
		<view v-for="(share,index) in shares" :key="index" @tap="auditDetail(share.id)" 
		class="flex flex-wrap diygw-col-24 flex-direction-column justify-center flex2-clz">
			<view class="flex diygw-col-24 justify-around avatar1-clz">
				<view class="diygw-avatar radius-md bg-none">
					<image mode="aspectFill" class="diygw-avatar-img radius-md" :src="share.cover"></image>
				</view>
			</view>
			<view class="flex flex-wrap diygw-col-24 flex4-clz">
				<view class="diygw-col-24 text4-clz"> {{share.title}} </view>
				<view class="diygw-text-line2 diygw-col-24 text5-clz"> {{share.summary}} </view>
			</view>
			<view class="flex flex-wrap diygw-col-4 flex1-clz">
				<view class="diygw-col-15 text-clz"> 去审核 </view>
				<image src="/static/yjt.png" class="response diygw-col-2 image-clz" mode="widthFix"></image>
			</view>
		</view>
		<view class="clearfix"></view>
	</view>
</template>

<script>
	import { request } from '@/utils/request'
	import { NOT_YET_SHARE_URL } from '@/utils/api'
	export default {
		data() {
			return {
				shares: []
			};
		},
		
		onLoad(option) {
			this.getShares();
		},
		onPullDownRefresh() {
			this.getShares();
		},
		methods: {
			async getShares() {
				let res = await request(NOT_YET_SHARE_URL,'GET',{});
				console.log(res);
				setTimeout(() => {
					uni.hideLoading();
				},1000);
				this.shares = res.data;
				uni.stopPullDownRefresh();
				if(res.success == true){
					this.shares = res.data;
					uni.stopPullDownRefresh();
				} else {
					uni.showToast({
						title: '没有权限访问'
					});
				}
			},
			auditDetail(id) {
				uni.navigateTo({
					url: `/pages/audit/resourceAudit?id=${id}`
				});
			}
		}
	};
</script>

<style lang="scss" scoped>
	.flex2-clz {
		margin-left: 10rpx;
		flex-shrink: 0;
		width: calc(100% - 10rpx - 10rpx) !important;
		margin-top: 10rpx;
		margin-bottom: 10rpx;
		height: 120rpx !important;
		text-align: center;
		margin-right: 10rpx;
	}
	.avatar1-clz {
		flex-shrink: 0;
		width: 100rpx !important;
	}
	.flex4-clz {
		flex-shrink: 0;
		width: 500rpx !important;
		height: 120rpx !important;
	}
	.text4-clz {
		flex-shrink: 0;
		font-weight: bold;
		width: 500rpx !important;
		font-size: 28rpx !important;
		text-align: left;
	}
	.text5-clz {
		flex-shrink: 0;
		text-align: left;
		width: 500rpx !important;
		font-size: 22rpx !important;
		margin-right: 10rpx;
	}
	.flex1-clz {
		margin-left: 2rpx;
		width: calc(16.6666666667% - 2rpx - 2rpx) !important;
		margin-top: 2rpx;
		margin-bottom: 2rpx;
		margin-right: 2rpx;
	}
	.text-clz {
		color: #000000;
		opacity: 0.5;
		text-align: left;
	}
	.image-clz {
		flex-shrink: 0;
		width: 40rpx !important;
		height: 40rpx !important;
	}
	.container25504 {
		padding-left: 0px;
		padding-right: 0px;
	}
	.container25504 {
	}
</style>
