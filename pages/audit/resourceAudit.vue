<template>
	<view class="container container25504">
		<u-navbar titleWidth="500" :isFixed="true" :isSlotTitle="true" title="资源审核" backIconColor="#fff" titleColor="inherit" bgColor="red" :backTextStyle="{ color: 'inherit' }" :isHome="false" :isBack="true">
			<template v-slot:title> 资源审核 </template>
		</u-navbar>
		<view class="flex flex-wrap diygw-col-24 flex-direction-column flex-clz">
			<view class="diygw-col-24 text1-clz"> {{share.title}} </view>
			<view class="flex flex-wrap diygw-col-24 flex1-clz">
				<view class="diygw-col-7 text-clz"> 作者：{{nickname}} </view>
				<view class="diygw-col-7 text4-clz"> 发布人：{{share.author}} </view>
				<view class="diygw-col-5 text3-clz"> 积分：{{share.price}} </view>
				<view v-if="share.isOriginal" class="diygw-col-5 text2-clz"> 来源：原创 </view>
				<view v-else class="diygw-col-5 text2-clz"> 来源：转载 </view>
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
		<view class="flex flex-wrap diygw-col-24 flex-direction-column">
			<u-form-item class="diygw-col-24 switch-clz" label="通过审核" prop="switch">
				<view class="flex diygw-col-24 justify-end">
					<u-switch :activeValue="1" :inactiveValue="0" activeColor="#0081ff" @tap="changeAuditStatus" v-model="pass" slot="right"></u-switch>
				</view>
			</u-form-item>
			<u-form-item class="diygw-col-24 switch1-clz" label="立即发布" prop="switch1">
				<view class="flex diygw-col-24 justify-end">
					<u-switch :activeValue="1" :inactiveValue="0" activeColor="#0081ff" @tap="showFlag" v-model="show" slot="right"></u-switch>
				</view>
			</u-form-item>
			<u-form-item class="diygw-col-24" label="原因" prop="input4" v-if="!pass">
				<u-input :focus="input4Focus" class="" placeholder="不通过审核的原因" v-model="downloadUrl" type="text"></u-input>
			</u-form-item>
			<view class="flex diygw-col-24">
				<button @tap="submit" class="diygw-btn red radius-xs flex-sub margin-xs button-button-clz">提交</button>
			</view>
		</view>
		<view class="clearfix"></view>
	</view>
</template>

<script>
import { SHARE_URL,AUDIT_URL } from '@/utils/api';
import { request } from '@/utils/request';

	export default {
		data() {
			return {
				shareId: '',
				share: {},
				nickname: '',
				avatarUrl: '',
				reason: '',
				pass: 1,
				show: 1
			};
		},
		onShow() {
			this.setCurrentPage(this);
		},
		onLoad(option) {
			this.shareId = option.id;
			console.log(this.shareId);
			this.getShare();
		},
		methods: {
			async getShare() {
				let id = this.shareId;
				uni.showLoading({
					title: '加载中'
				});
				let res = await request(SHARE_URL + `/${id}`,'GET');
				setTimeout(() => {
					uni.hideLoading();
				},500);
				console.log(res.data);
				this.share = res.data.share;
				console.log(this.share);
				this.nickname = res.data.nickname;
				this.avatarUrl = res.data.avatarUrl;
			},
			changeAuditStatus(evt) {
				console.log(this.showFlag)
			},
			changeShowFlag(evt) {
				console.log(this.auditStatus)
			},
			async submit() {
				 uni.showLoading({
				   title: '审核中'
				 });
				 let data = await request(AUDIT_URL + `/${this.shareId}`, 'POST', {
				   auditStatusEnum: this.pass == 1 ? 'PASS' : 'REJECT',
				   reason: this.pass ? '通过审核' : this.reason,
				   showFlag: this.show,
			 });
			 setTimeout(() => {
			   uni.hideLoading();
			 }, 500);
			 if (this.pass === 1) {
				uni.showToast({
					title:'审核通过',
					duration:500
				})
				uni.switchTab({
					url:'/pages/index/index'
			   });
			 }
		}
	},
}
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
	.text2-clz {
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
	.text6-clz {
		margin-left: 10rpx;
		width: calc(100% - 10rpx - 10rpx) !important;
		font-size: 28rpx !important;
		margin-top: 10rpx;
		margin-bottom: 10rpx;
		text-align: left;
		margin-right: 10rpx;
	}
	.switch-clz {
		font-size: 28rpx !important;
	}
	.switch1-clz {
		font-size: 28rpx !important;
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
