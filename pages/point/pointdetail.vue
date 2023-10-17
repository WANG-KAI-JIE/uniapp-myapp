<template>
	<view class="container container25504">
		<u-navbar titleWidth="500" :isFixed="true" :isSlotTitle="true" title="积分明细" backIconColor="#fff" titleColor="inherit" bgColor="red" :backTextStyle="{ color: 'inherit' }" :isHome="false" :isBack="true">
			<template v-slot:title> 积分明细 </template>
		</u-navbar>
		<view v-for="(myValue,index) in myValues" :key="index">
			<view v-if="myValue.value<0" class="flex flex-wrap diygw-col-24 justify-end flex-clz">
				<text class="flex icon diygw-col-2 icon-clz diy-icon-youhuiquan"></text>
				<view class="flex flex-wrap diygw-col-19">
					<view class="diygw-col-12 mg"> {{myValue.createTime}} </view>
					<view class="diygw-col-3"> —— </view>
					<view class="diygw-col-5"> {{myValue.value}} </view>
				</view>
				<text class="flex icon1 diygw-col-2 icon1-clz diy-icon-pullright"></text>
			</view>
			<view v-else class="flex flex-wrap diygw-col-24 justify-end flex2-clz">
				<text class="flex icon2 diygw-col-2 icon2-clz diy-icon-shouru"></text>
				<view class="flex flex-wrap diygw-col-19">
					<view class="diygw-col-12 mg"> {{myValue.createTime}} </view>
					<view class="diygw-col-3"> —— </view>
					<view class="diygw-col-5"> +{{myValue.value}} </view>
				</view>
				<text class="flex icon3 diygw-col-2 icon3-clz diy-icon-pullright"></text>
			</view>
		</view>
		
		<view class="clearfix"></view>
	</view>
</template>

<script>
	import { request } from '@/utils/request'
	import { MYVALUE } from '@/utils/api'
	export default {
		data() {
			return {
				myValues: [],
			};
		},
		onLoad(option) {
			this.getValues();
		},
		onPullDownRefresh() {
			this.getValues();
		},
		methods: {
			async getValues() {
				let res = await request(MYVALUE,'GET',{});
				console.log(res);
				this.myValues = res.data;
			},
		}
	};
</script>

<style lang="scss" scoped>
	.mg {
		margin: auto;
		margin-left: 50rpx;
	}
	.flex-clz {
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
	.icon-clz {
		color: #5bc6a7;
		text-align: left;
	}
	.icon {
		font-size: 40rpx;
	}
	.icon1-clz {
		text-align: right;
	}
	.icon1 {
		font-size: 40rpx;
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
	.icon2-clz {
		color: #db6055;
		text-align: left;
	}
	.icon2 {
		font-size: 40rpx;
	}
	.icon3-clz {
		text-align: right;
	}
	.icon3 {
		font-size: 40rpx;
	}
	.container25504 {
		padding-left: 0px;
		padding-right: 0px;
	}
	.container25504 {
	}
</style>
