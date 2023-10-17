<template>
	<view class="container container25504">
		<u-navbar titleWidth="500" :isFixed="true" :isSlotTitle="true" title="我的投稿" backIconColor="#fff" titleColor="inherit" bgColor="red" :backTextStyle="{ color: 'inherit' }" :isHome="false" :isBack="true">
			<template v-slot:title> 我的投稿 </template>
		</u-navbar>
		<view v-for="(share,index) in shares"  :key="index" class="flex flex-wrap diygw-col-24 flex-direction-column justify-center flex2-clz">
			<view class="flex diygw-col-24 justify-around avatar-clz">
				<view class="diygw-avatar radius-md bg-none">
					<image mode="aspectFill" class="diygw-avatar-img radius-md" :src="share.cover"></image>
				</view>
			</view>
			<view class="flex flex-wrap diygw-col-8 flex4-clz">
				<view class="diygw-col-9 text4-clz">  {{share.title}} </view>
				<view class="diygw-text-line2 diygw-col-13 text5-clz"> {{share.summary}} </view>
			</view>
			<view class="flex flex-wrap diygw-col-8 flex1-clz">
				<view class="diygw-col-10 text-clz"> {{share.auditStatus}} </view>
				<image src="/static/yjt.png" class="response diygw-col-1 image-clz" mode="widthFix"></image>
			</view>
		</view>
		<view class="clearfix"></view>
	</view>
</template>

<script>
	import {
		request
	} from "@/utils/request"
	
	import {
		MY_CONTRIBUTION_URL
	} from '@/utils/api'
	export default {
		data() {
			return {
				shares: [],
				pageNo: 1,
				pageSize: 10,
				more: true,
				statusInfo: '',
				//用户全局信息
				userInfo: {},
				//页面传参
				globalOption: {},
				//自定义全局变量
				globalData: {}
			};
		},
		onLoad(option) {
			this.getShares(true);
		},
		onReachBottom() {
			
		},
		onPullDownRefresh() {
			
		},
		methods: {
			async getShares(init) {
				if(init) {
					this.pageNo = 1;
					this.more = true;
				}
				uni.showLoading({
					title: '加载中'
				});
				let res = await request(MY_CONTRIBUTION_URL,'GET', {
					pageNo: this.pageNo,
					pageSize: this.pageSize
				});
				setTimeout(() => {
					uni.hideLoading();
				},100);
				if(init) {
					this.shares = res.data;
					uni.stopPullDownRefresh();
				} else {
					this.shares = this.shares.concat(res.data);
				}
				if(res.data.length < this.pageSize && this.pageNo > 0) {
					this.more = false;
				}
				let shares = res.data;
				this.shares = shares.map(function(item) {
					let res = {};
					switch (item.auditStatus) {
						case 'NOT_YET': 
							res.auditStatus = '尚未审核';
							break;
						case 'PASS':
							res.auditStatus = '审核通过';
							break;
						case 'REJECT':
							res.auditStatus = '审核被拒';
							break;
					}
					res.id = item.id;
					res.title = item.title;
					res.cover = item.cover;
					res.summary = item.summary;
					setTimeout(() => {
						uni.hideLoading();
					},100);
					return res;
				});
			},
		}
	};
</script>

<style lang="scss" scoped>
	.flex2-clz {
		margin-left: 30rpx;
		flex-shrink: 0;
		width: calc(100% - 10rpx - 10rpx) !important;
		margin-top: 10rpx;
		margin-bottom: 10rpx;
		height: 120rpx !important;
		text-align: center;
		margin-right: 10rpx;
	}
	.avatar-clz {
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
		width: 500rpx !important;
		font-size: 22rpx !important;
		text-align: left;
	}
	.flex1-clz {
		margin-left: 2rpx;
		padding-top: 2rpx;
		padding-left: 2rpx;
		width: calc(33.3333333333% - 2rpx - 2rpx) !important;
		padding-bottom: 2rpx;
		margin-top: 2rpx;
		margin-bottom: 2rpx;
		margin-right: 2rpx;
		padding-right: 2rpx;
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
		text-align: left;
	}
	.container25504 {
		padding-left: 0px;
		padding-right: 0px;
	}
	.container25504 {
	}
</style>
