<template>
	<view class="container container25504">
		<view class="flex flex-wrap diygw-col-24 flex-direction-column justify-center">
			<view class="diygw-col-24 text-clz"> 说明：投稿审核通过后会有积分奖励；资源被下载会有积分奖励；提交的资源不得包含广告、侵权信息，百度盘地址建议有密码。 </view>
		</view>
		<view class="flex flex-wrap diygw-col-24 flex-direction-column justify-center">
			<view class="flex flex-wrap diygw-col-24 justify-center">
				<u-form-item class="diygw-col-24" label="来源" prop="radio">
					<u-radio-group class="flex flex-wrap diygw-col-24 justify-between" wrapClass=" justify-between" activeColor="#e54d42" v-model="radio" @change="changeRadio">
						<u-radio class="diygw-col-12" shape="circle" v-for="(radioitem, radioindex) in radioDatas" :key="radioindex" :name="radioitem.value">
							{{ radioitem.label }}
						</u-radio>
					</u-radio-group>
				</u-form-item>
			</view>
			<u-form-item class="diygw-col-24" label="标题" prop="input">
				<u-input :focus="inputFocus" class="" placeholder="请输入标题" v-model="title" type="text"></u-input>
			</u-form-item>
			<u-form-item class="diygw-col-24" label="作者" prop="input2">
				<u-input :focus="input2Focus" class="" placeholder="请输入作者" v-model="author" type="text"></u-input>
			</u-form-item>
			<u-form-item class="diygw-col-24" label="价格" prop="input1">
				<u-input :focus="input1Focus" class="" placeholder="请输入价格" v-model="price" type="text"></u-input>
			</u-form-item>
			<u-form-item class="diygw-col-24" label="简介" prop="input3">
				<u-input :focus="input3Focus" class="" placeholder="介绍一下技术干货吧" v-model="summary" type="text"></u-input>
			</u-form-item>
			<u-form-item class="diygw-col-24 input4-clz" label="封面图" prop="input4">
				<u-input :focus="input4Focus" class="" placeholder="请输入封面图网址" v-model="cover" type="cover"></u-input>
			</u-form-item>
			<u-form-item class="diygw-col-24 input5-clz" label="下载地址" prop="input5">
				<u-input :focus="input5Focus" class="" placeholder="请输入网盘链接" v-model="downloadUrl" type="text"></u-input>
			</u-form-item>
			<view class="flex diygw-col-24">
				<button @tap="submit" class="diygw-btn red radius-xs flex-sub margin-xs button1-button-clz">提交</button>
			</view>
		</view>
		<view class="clearfix"></view>
	</view>
</template>

<script>
	import { request } from '@/utils/request';
	import { CONTRIBUTE_URL } from '@/utils/api'
	export default {
		data() {
			return {
				title: '美食图鉴大赏',
				author: 'kjwang',
				isOriginal: 1,
				price: 10,
				summary: "很棒的美食图鉴大赏",
				cover: "https://fatcat666.oss-cn-nanjing.aliyuncs.com/image/202310150932551.png",
				downloadUrl: "https://pan.xxx.com/**.zip",
				//用户全局信息
				userInfo: {},
				//页面传参
				globalOption: {},
				//自定义全局变量
				globalData: {},
				radioDatas: [
					{ value: '1', label: '原创', checked: true },
					{ value: '0', label: '转载', checked: false }
				],
				radio: '1',
				inputFocus: false,
				input: '',
				input2Focus: false,
				input2: '',
				input1Focus: false,
				input1: '',
				input3Focus: false,
				input3: '',
				input4Focus: false,
				input4: 'http://XXX',
				input5Focus: false,
				input5: '链接：http://pan.XXX'
			};
		},
		onShow() {
		},
		onLoad(option) {
		},
		methods: {
			async submit() {
				uni.showLoading({
					title: '投稿中'
				});
				let res = await request(CONTRIBUTE_URL, 'POST',{
					isOriginal: this.isOriginal == 1 ? true : false,
					title: this.title,
					author: this.author,
					price: this.price,
					cover: this.cover,
					summary: this.summary,
					downloadUrl: this.downloadUrl
				});
				setTimeout(() => {
					uni.hideLoading();
				},1000);
				console.log(res)
				if(res === 1) {
					// res.data
					uni.showModal({
						title: '提示',
						content: '投稿成功，将在最短的时间内审核资源！',
						cancalText: '返回首页',
						confirmText: '继续投稿',
						success(res) {
							if(res.confirm){
								this.isOriginal = false;
								this.title = '';
								this.author = '';
								this.price = '';
								this.summary = '';
								this.downloadUrl = '';
							} else {
								uni.switchTab({
									url: '/pages/index/index'
								});
							}
						}
					})
				}
			}
		}
	};
</script>

<style lang="scss" scoped>
	.text-clz {
		margin-left: 10rpx;
		width: calc(100% - 10rpx - 10rpx) !important;
		font-size: 28rpx !important;
		margin-top: 10rpx;
		margin-bottom: 10rpx;
		text-align: left;
		margin-right: 10rpx;
	}
	.input4-clz {
		font-weight: bold;
	}
	.input5-clz {
		font-weight: bold;
	}
	.button1-button-clz {
		margin: 3px !important;
	}
	.container25504 {
		padding-left: 0px;
		padding-right: 0px;
	}
	.container25504 {
	}
</style>
