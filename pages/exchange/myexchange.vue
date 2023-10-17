<template>
	<view class="container container25504">
		<view class="flex flex-wrap diygw-col-24 flex-direction-column"> </view>
		<u-navbar titleWidth="500" :isFixed="true" :isSlotTitle="true" title="我的兑换" backIconColor="#fff" titleColor="inherit" bgColor="red" :backTextStyle="{ color: 'inherit' }" :isHome="false" :isBack="true">
			<template v-slot:title> 我的兑换 </template>
		</u-navbar>
		<view class="flex diygw-col-24 flex-direction-column">
			<view class="">
					<view v-for="(share,index) in shares" :key="index">
						<view class="flex flex-wrap diygw-col-24 flex-direction-column justify-center flex1-clz">
							<view class="flex diygw-col-24 justify-around avatar-clz">
								<view class="diygw-avatar radius-md bg-none">
									<image mode="aspectFill" class="diygw-avatar-img radius-md" :src="share.cover"></image>
								<text 
									style="position: absolute;left: -15px;top: -5px;background-color: red;font-size: 13px;padding: 0 5px;"
									v-if="share.isOriginal">原创</text>
								<text v-else
									style="position: absolute;left: -15px;top: -5px; background-color: green;font-size: 13px;padding: 0 5px;">
									转载</text>
								</view>
							</view>
							<view class="flex flex-wrap diygw-col-24 flex-clz">
								<view class="diygw-col-24 text-clz"> {{share.title}} </view>
								<view class="diygw-text-line2 diygw-col-24 text1-clz"> {{share.summary}} </view>
							</view>
							<view class="flex flex-wrap diygw-col-24 flex-direction-column justify-center flex3-clz">
								<view class="diygw-col-24 text3-clz"> {{share.price}} 积分 </view>
								<view class="diygw-col-24" v-if="share.downloadUrl" @tap="gotoDetail(share.id)"> 下载 </view>
								<view class="diygw-col-24" v-else @tap="gotoExchange(share.id)"> 兑换 </view>
							</view>
						</view>
						<view class="flex diygw-col-24">
							<view class="diygw-pzx" style="border-bottom: 1px solid #eee"></view>
						</view>
				</view>
			</view>
		</view>
		<view class="clearfix"></view>
	</view>
</template>

<script>
		import {
			request,	
		} from '@/utils/request';
		
		import{
			MY_EXCHANGE_URL
		} from '@/utils/api';
		
	export default {
		data() {
			return {
				shares: []
			};
		},
		onLoad(option) {
			this.getMyExchange();
		},
		// 下拉刷新
		onPullDownRefresh() {
			// 参数为 true，就重新开始加载
			this.getShares(true);
		},
		// 触底加载更多
		onReachBottom() {
			// 没有更多数据了
			if(!this.more){
				uni.showToast({
					title: '已加载完毕',
					duration: 1000
				});
				// 直接返回，不执行下面代码了
				return;
			}
			// 正常加载下一页
			this.pageNo = this.pageNo + 1;
			uni.showLoading({
				title: '加载中'
			});
			// 不带参数的请求，就是分页加载
			this.getShares();
			setTimeout(() => {
				uni.hideLoading();
			}, 1000);
		},
		
		methods: {
			async getMyExchange(){
				let res = await request(MY_EXCHANGE_URL,'GET',{});
				console.log(res)
				this.shares = res.data;
			},
			gotoDetail(id) {
				uni.navigateTo({
					url: "/pages/detail/detail?id=" + id
				})
			},
		}
	};
</script>

<style lang="scss" scoped>
	.tabs-item-title.cur {
		color: #db6055 !important;
	}
	.noticebar-clz {
		text-shadow: 1px 1px 2px #333;
		font-size: 20rpx !important;
	}
	.flex1-clz {
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
	.flex-clz {
		flex-shrink: 0;
		width: 500rpx !important;
		height: 120rpx !important;
	}
	.text-clz {
		flex-shrink: 0;
		font-weight: bold;
		width: 500rpx !important;
		font-size: 28rpx !important;
		text-align: left;
	}
	.text1-clz {
		flex-shrink: 0;
		width: 500rpx !important;
		font-size: 22rpx !important;
		text-align: left;
	}
	.flex3-clz {
		flex-shrink: 0;
		width: 120rpx !important;
		text-align: center;
		margin-right: 5rpx;
	}
	.text3-clz {
		flex-shrink: 0;
		font-weight: bold;
		width: 140rpx !important;
		height: 50rpx !important;
	}
	.text2-clz {
		flex-shrink: 0;
		width: 140rpx !important;
		height: 50rpx !important;
	}
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
		width: 500rpx !important;
		font-size: 12rpx !important;
	}
	.flex5-clz {
		flex-shrink: 0;
		width: 120rpx !important;
		text-align: center;
	}
	.text6-clz {
		flex-shrink: 0;
		font-weight: bold;
		width: 140rpx !important;
		height: 50rpx !important;
	}
	.text7-clz {
		flex-shrink: 0;
		width: 140rpx !important;
		height: 50rpx !important;
	}
	.flex8-clz {
		flex-shrink: 0;
		width: 400rpx !important;
	}
	.flex6-clz {
		margin-left: 10rpx;
		width: calc(100% - 10rpx - 10rpx) !important;
		margin-top: 10rpx;
		margin-bottom: 10rpx;
		text-align: left;
		margin-right: 10rpx;
	}
	.text8-clz {
		flex-shrink: 0;
		padding-top: 10rpx;
		font-weight: bold;
		padding-left: 10rpx;
		width: 100rpx !important;
		font-size: 36rpx !important;
		padding-bottom: 10rpx;
		height: 100rpx !important;
		text-align: center;
		padding-right: 10rpx;
	}
	.flex7-clz {
		padding-top: 10rpx;
		flex: 1;
		padding-left: 10rpx;
		padding-bottom: 10rpx;
		padding-right: 10rpx;
	}
	.text9-clz {
		color: #db6055;
		font-weight: bold;
		flex: 1;
		font-size: 32rpx !important;
	}
	.line2-clz {
		flex: 1;
	}
	.container25504 {
		padding-left: 0px;
		padding-right: 0px;
	}
	.container25504 {
	}
</style>
